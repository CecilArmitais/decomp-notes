# `0f3cbcbc` — Decompile 26 more callees; clear three asm files entirely

| | |
|---|---|
| **Commit** | `0f3cbcbc` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `967fe53b` |
| **Verified** | all three ROMs: `pmdsky.us.nds: OK`, `pmdsky.eu.nds: OK`, `pmdsky.jp.nds: OK` |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The PR diff and the matching build are the sources
> of truth — see [the README](../README.md). Claims are labelled **fact** (read
> off the asm/data, or from an existing in-tree header) or **inference**.

---

This closes the three near-match groups left over from the previous batch. All
26 remaining functions reach score 0, so three whole `.s` files clear and their
objects leave `main.lsf`.

**It is not a 26-function commit that happens to touch headers.** It carries
**three shared-type changes** and a **declaration cleanup that found a real
type error in eight places**. Those four items are what a reviewer should spend
their time on; the function list is the easy part. They are each argued below,
before the per-group detail.

## What landed, and where

**Fact**, read out of the diff and `main.lsf`.

| asm file (deleted) | fns | destination |
|---|---|---|
| `asm/overlay_29_02337EC0.s` | 6 | `RemoveMonsterFromTile`, `ov29_02337EE8`, `GetRandomSpawnMonsterID` → **`src/dungeon_map_access_1.c`** (head merging backwards); `ov29_0233804C`, `ov29_023380FC`, `ov29_023381C0` → top of **`src/overlay_29_02338350.c`** |
| `asm/overlay_29_022E335C.s` | 10 | nine appended to **`src/dg_camera.c`** in address order; `PlayEffectAnimationEntity` prepended to **`src/dg_effect.c`** above `EntityIsValid__022E37B8` |
| `asm/overlay_29_0230F02C.s` | 10 | all ten prepended to **`src/overlay_29_0230F810.c`**, in address order, before `ItemIsActive__0230F810` |

Each `.s` contained exactly those functions and nothing else (fact — per-group
address walks close exactly on the next object's first symbol). `main.lsf`
loses three `Object` lines and gains none: no file split, no new object, no new
header file. Three `asm/include/*.inc` files go with them.

**One generalisable cost, worth carrying forward.** In the 022E335C group the
last straggler, `ov29_022E34C8`, sat in the *middle* of the file. While it was
unmatched the plan required splitting the `.s`, creating
`src/overlay_29_022E3534.c` + `include/overlay_29_022E3534.h`, and adding
objects to `main.lsf`. The moment it matched, all of that vanished and the head
merged backwards into one existing file. A straggler in the middle of a
contiguous `.s` costs far more than its own function; one at either end costs
almost nothing.

---

# 1. Shared-type change: `struct unk_02337EE8` in `include/dungeon.h`

Ten consecutive `struct dungeon` members — `group_id_copy` (0x286B0) through
`spawn_table_entries_chosen` — are regrouped into one named sub-object, and the
parent gains `struct unk_02337EE8 field_0x286b0;` in their place. Eleven
expressions in four files gain `field_0x286b0.` after the `->`.

Evidence, layout proof and the region map: `wip/overlay_29_02337EC0_all/cand/census.md`
and `cand/census_dungeon_h.txt`.

### Why the tree had to change at all

**Fact.** `ov29_02337EE8` and `GetRandomSpawnMonsterID` were byte-identical to
their targets **except for four instructions each** — the materialisation of
`&dungeon->spawn_entries_master[0]`, score 225 on both:

```
target                          candidate
add r0, r0, #0x6b0              add r0, r0, #0x164
add r0, r0, #0x28000            add r8, r0, #0x2c800
add r0, r0, #0x2b4
add r8, r0, #0x4000
```

Both reach `dungeon + 0x2C964`. **Fact:** `0x6b0 + 0x28000 = 0x286B0` and
`0x2b4 + 0x4000 = 0x42B4`, and `0x286B0 + 0x42B4 = 0x2C964`. **Fact:** mwcc's
constant-add splitter is LSB-first, 8 bits per chunk at an even rotation, and
goes to a literal pool word at three chunks — re-derived with the real compiler
in `probe/addr.c` and checked against five known offsets. No other contiguous
partition of the target's four adds is consistent with that, so retail
performed **two** pointer additions and the first lands on 0x286B0.

**Fact, region corroboration.** JAPAN emits `#0x20c + #0x28400` (= 0x2860C)
then the *same* `#0x2b4 + #0x4000`. Only the first constant moves between
regions — which is exactly what a member offset into `struct dungeon` followed
by a fixed offset from there would look like. Six independent sites carry the
same −0xA4 JP shift for this block.

### The shape table — both halves are required

**Fact** (measured, `ov29_02337EE8`, NORTH_AMERICA, real target, baseline 225):

| shape | score |
|---|---|
| sub-object alone, inline `dungeon->field_0x286b0.spawn_entries_master[i]` | **225 — unchanged** |
| local pointer alone, no sub-struct (LEDGER items 7/9) | **225** |
| **sub-object + `struct unk_02337EE8 *p = &dungeon->field_0x286b0;` then `&p->spawn_entries_master[i]`** | **0** |

Neither half does anything on its own. That is the whole finding, and it is why
the header change is not optional cosmetics.

**Fact, the block's start is forced, not chosen** — same body, same everything
else, only the sub-object's start point varied:

| block starts at | emitted adds | score |
|---|---|---|
| **0x286B0 `group_id_copy`** | `#0x6b0, #0x28000, #0x2b4, #0x4000` | **0** |
| 0x286B1 `field_0x286b1` | `#0x2b4, #0x4000` | 330 |
| 0x286B2 `floor_properties` | `#0xb2, #0x4200` | 340 |
| 0x286D2 `item_spawn_weights` | `#0x92, #0x4200` | 340 |

That agrees with an independent fact: `LoadMappaFileAttributes`
(`asm/overlay_29_022E6928.s:506`) begins its writes at exactly 0x286B0 and is
the only writer of the whole range.

### The spelling is already in the tree

**Fact.** `src/overlay_29_022FBBEC.c:72` already contains
`struct unk_022FBD24 *p = &DUNGEON_PTR[0]->field_0x3dcc;`, character for
character, for the *other* grouped member of this same struct (landed in
pmd-sky `964beafb`). Nothing about the idiom is invented here.

### Safety of the edit

* **Layout-preserving.** `sizeof(struct dungeon)` plus 17 offsets in and around
  the block, asked of the real compiler flat and nested, in all three regions:
  **nothing moved**. *(Fact — `cand/census_verify.py` step 1.)*
* **Byte-neutral for existing code.** All 11 other expressions naming a member
  of the block compile to **byte-identical** emission both ways in all three
  regions *(fact, step 2)*. They are still **not optional** — the tree does not
  compile without them. Measured in the diff: `src/overlay_29_022EFA6C.c` ×1,
  `src/random_trap.c` ×1, `src/spawn_1.c` ×8, `src/spawn_2.c` ×1 = 11.
* No `asm/` file names a struct member, so `asm/` needed no change *(fact)*.
* **Comments inside the new struct are MOVED VERBATIM** from the members they
  already sat on — CLAUDE.md's one mechanical exception. No comment there is
  authored.

### What is inference here, and one unresolved tension

**Inference:** that the block is "the floor's attributes as loaded from the
mappa file". `LoadMappaFileAttributes` writing all of it is a fact; the meaning
is not, which is why the type stays `struct unk_02337EE8` and claims nothing.

**Inference:** the block's *end*. Its start is forced (above); 0x2CA0A/0x2CA0C
is a judgement call — 0x2CA0C is where an unrelated concern begins
(`CreateMonsterSummaryFromEntityOuter` bases a pointer there).

**Not checked:** there is no second consumer to corroborate the sub-object's
shape. `0x42B4` appears **nowhere else in the ROM** in either region (fact,
swept), so the boundary had to be pinned by measurement rather than by a
cross-reference.

**A tension a reviewer may want to pin down, which nothing I read resolves.**
`wip/overlay_29_02337EC0_all/LEDGER.md` item 10 records "a nested sub-struct in
the header — the obvious hypothesis, and it is wrong", from `probe/t.c`, where
`&BIGP->sub` followed by `s->arr` **folded to one offset**. That is
structurally the shape that later scored 0. The two measurements differ in two
ways at once — `probe/t.c` used a synthetic `struct big_t` of padding arrays and
only *returned* the address, whereas the census measured the real header inside
the real loop with LICM — and **which difference accounts for it was not
isolated**. It does not affect the match, which is measured end to end in all
three regions; it does mean LEDGER item 10 should not be read as a general
result.

**Also worth a reviewer's eye:** the ledger records a spelling that reproduces
the target's four adds exactly and reaches 0 differing rows — a pointer cast
sitting between the two additions, `(struct monster_spawn_entry *)((u8 *)&DUNGEON_PTR->group_id_copy + 0x42B4)`.
It was **deliberately not shipped**: it hard-codes 0x42B4 and takes the address
of an unrelated member purely for its numeric offset, which is not a
decompilation. The honest spelling was found afterwards, and it is what landed.

### Divergence from pmdsky-debug

**Fact**, recorded in the ledger: pmdsky-debug's `struct dungeon` is **flat**
here too (`headers/types/dungeon_mode/dungeon.h`), so this sub-object is a
deviation from upstream's shape, not a port of it. The offsets agree; the
grouping is new. That is a sync question for whoever next runs
`sync_to_pmdsky_debug.py`.

---

# 2. Shared-type change: two width corrections

Both replace **adjacent `u8` members with one wider member at the same
offset**, so no following offset and no struct alignment moves *(fact — the
members are contiguous and the wider type is naturally aligned there; confirmed
by all three builds)*.

### `dungeon::number_completed_floors` — `u8` → `s16`, absorbing `speed_boost_counter` at 0x1F

**Fact, the reads.** `ldrsh r1, [r1, #0x1e]` in `ov29_022E335C` itself, in
`DisplayFloorCard` (`asm/overlay_29_02348020.s:223`), and at
`asm/overlay_29_0233544C.s:583` and `:1019` — all of them pairing it with
`ldrb [dungeon, #0x749]`, i.e. `floor`. A `u8` field gives `ldrb`, which does
not match.

**Fact, the write.** `strh` at `asm/overlay_29_022E6928.s:545`, whose next three
instructions compute `[0x22] = [0x20] + [0x1e]` — exactly what `dungeon.h`
already documents `total_floors_completed` as being set to.

**Fact, blast radius.** Post-landing grep of `src/` and `include/` for both
names finds only the declaration, its comment, and the one new use at
`src/dg_camera.c:88`. `speed_boost_counter` has no remaining users anywhere.

**Two documentation casualties, flagged and deliberately not fixed** (CLAUDE.md
forbids authoring comments in `pmd-sky`; neither was mine to re-word):

* Deleting `u8 speed_boost_counter;` deleted **its comment** — *"0x1F: Turn
  counter, Speed Boost triggers every 250 turns, then the counter is reset."*
  That was the tree's only record of what 0x1F is, and 0x1F is now inside the
  16-bit field. The behaviour is unaffected by a width change; the description
  of it is gone.
* The surviving comment on `number_completed_floors` still reads *"odd it is not
  a u16 like the others"* — written when the member was `u8`, now stale against
  the `s16` beneath it. (It is also the comment that made the width worth
  doubting, so it earned its keep.)

**A reviewer with full context should decide what those comments become.**

#### Addendum (2026-09-19): the Speed Boost counter is real — it lives on `struct monster`, not at `dungeon` 0x1F

Written while preparing the branch to be pushed. It **resolves** the open
question above, and moves the merge from *"the reads force 16 bits"* to *"0x1F
was never a field."*

**Fact — the mechanic upstream describes exists, at a different address.**
`ActivateEndOfTurnEffects` (`asm/overlay_29_0230F9A4.s:766-784`) gates on ability
`0xB` and then increments, tests and resets a **byte on `struct monster`**:

```asm
mov  r1, #0xb                                      ; Speed Boost
bl   AbilityIsActiveVeneer
ldrb r1, [r4, #0x11f + OV29_0230FC24_OFFSET]       ; r4 = entity->info
ldr  r0, _02310AA8 ; =SPEED_BOOST_TURNS
add  r2, r1, #1
strb r2, [r4, #0x11f + OV29_0230FC24_OFFSET]       ; counter + 1
cmp  r1, r0 / blt …
strb r3, [r4, #0x11f + OV29_0230FC24_OFFSET]       ; reset to 0
bl   BoostSpeedOneStage
```

and `SPEED_BOOST_TURNS` is `.byte 0xFA` = **250**
(`asm/overlay_10_rodata_022C464C.s`). That is upstream's sentence — *"Turn
counter, Speed Boost triggers every 250 turns, then the counter is reset"* —
matched term for term.

`OV29_0230FC24_OFFSET` is the file's own region macro, defined at
`asm/overlay_29_0230F9A4.s:201-209` as `-4` under `JAPAN` and `0` otherwise, so
the member is **`monster + 0x11F` in NORTH_AMERICA and EUROPE** and `0x11B` under
JAPAN. `include/dungeon_mode.h:405` already declares `u8 field_0x11f;` — the byte
exists in the tree today, unnamed, and is the one being counted.

**Inference (well-supported).** pmdsky-debug attached a correctly-described, real
mechanic to the wrong address. The comment deleted with the member documented
behaviour that does not live at `dungeon` 0x1F, which is why deleting the member
cost no reader anything.

**Fact — nothing accesses `dungeon` 0x1D or 0x1F, at any width.** Every
immediate-offset byte access at `#0x1d` (80 sites) and `#0x1f` (38 sites) in the
whole `a3d64122` asm tree was enumerated and classified. A member at a fixed
offset can only be reached by an immediate-offset `ldrb`/`strb` (the ARM
byte-immediate range is 0–4095), so for a fixed field the census is complete.
Every site resolves to one of: `struct monster` 0x1A–0x1D — the four vitamin
stat-boost bytes, identified one-to-one by `ApplyProteinEffect` /
`ApplyCalciumEffect` / `ApplyIronEffect` / `ApplyZincEffect` in
`asm/overlay_29_02317844.s`; a 0x1E-byte dungeon-generation grid cell (stride
`0x1C2` = 15 × `0x1E`); a stack local; or a DWC/menu/keyboard/sound record
outside dungeon mode. **Zero** are `struct dungeon`.

**Fact — alignment forces the direction of the merge.** A 16-bit member must be
2-byte aligned: 0x1E is even, 0x1F is odd, so the halfword can only begin at
0x1E. It is not possible for `speed_boost_counter` to have been the 16-bit field
with `number_completed_floors` absorbed into it. The merge could only go this way
round.

**Fact — the initialisation writes both bytes at once.**
`asm/overlay_29_022E6928.s:545` is `strh r6, [r0, #0x1e]` with `r6 = 0`, which
zeroes 0x1E **and** 0x1F together. Had 0x1F been an independent turn counter,
loading a dungeon would have silently reset it every time.

**Deliberately still not established: the name.** What is proved is that the
field at 0x1E is 16-bit, is read signed, is zeroed at dungeon load, is summed
with 0x20 into 0x22, and is added to the floor byte at 0x749 to form a displayed
floor number. `number_completed_floors` remains upstream's guess. Mildly against
it: it is 0x1E, not 0x20, that is added to the floor number in `ov29_022E335C`,
`DisplayUi` and `DisplayFloorCard`, which sits oddly with the upstream
descriptions of *both* members. Renaming needs its own investigation; the merge
does not depend on the name being right.

**Coverage caveat, stated as a limit rather than a result.** ~95 of the censused
sites lie outside dungeon-mode code and were classified **structurally**, not by
hand-tracing each base register: their files contain no `=DUNGEON_PTR`, none of
the enclosing functions is `bl`-ed from any `overlay_29` file (nor from
`asm/main_0204357C.s` or `asm/overlay_31_023838E4.s`), and the subsystems are
DWC/GameSpy, Sha1, sound, keyboard, windowing, menus and `vsprintf`. The two that
*did* have a dungeon-side call edge were traced individually — `ShowKeyboard`
(`asm/main_02034974.s:2334-2342`, base is the `_020AFDF0` keyboard global) and
`CreateAdvancedMenu` (its 0x1f accesses are `[sp, #0x1f]` stack locals). No
prototype under `include/` takes a `struct dungeon *` at all, so a dungeon
pointer arriving in one of those files as a parameter is improbable — but that is
an argument, not a proof.

**A method note worth keeping.** The first census run was **unsound**, and in the
worst possible place: the site regex spelled the base as `\[r[0-9a-z]+,`, which
excludes `sp`, `sl`, `sb`, `fp`, `ip` and `lr`, and matched `(ldrb|strb)[a-z]*`,
which cannot match a condition code written before the `b` (`strneb`, `ldreqb`).
Callee-saved registers are exactly where a long-lived dungeon pointer lives, so
the blind spot covered the case the census existed to rule out. The negative
above is the re-derived one. This is the same failure as
`docs/MATCHING_TIPS.md` → *A search harness that cannot fail loudly will hand you
a perfect wrong answer*, reached from a third direction.

**A prediction, for whoever lands `LoadMappaFileAttributes`.**
`asm/overlay_29_022E6928.s:547` reads `dungeon + 0x20` with **`ldrsh`**, while
the tree declares `u16 number_preceding_floors`. MWCC emits `ldrh` for a `u16`
rvalue, so expect to need `s16` there. Nothing contradicts `u16` today only
because that function is still assembly and has never been compiled. 0x22's
signedness stays undetermined — it is only ever stored in the code found, and
`strh` is emitted for both signednesses.

### `monster::field_0x188` — four `u8` → one `s32`

**Fact, the reads.** `PlayEffectAnimationEntity` reads it with a word `ldr`
twice. `asm/overlay_29_02318AD4.s:30,73` reads it `ldr` and compares against
`0xC800`, immediately after testing `two_turn_move_invincible == 1`.

**Fact, the write.** `str r5, [r4, #0x188]` at `asm/overlay_29_022E0378.s:215`,
where `r4` is unambiguously a monster (`ldrsh r0,[r4,#2]` / `strh r0,[r4,#4]` /
`strb r5,[r4,#0x106]` around it). `0x188` is 4-aligned.

**Inference:** that `0xC800` is `200 << 8`, an 8.8 fixed-point pixel value. The
comparison is a fact; that reading of the constant is not, and nothing depends
on it.

**Fact, regions.** These four members sit **outside** every region `#ifdef` in
`struct monster`, so one edit serves all three — the JP arm of that block reads
`[r6, #0x184]`, the same field after the earlier `#ifndef JAPAN` members.

**Fact, blast radius.** Grep for all four names hits only `src/main_0203D538.c`,
which has an unrelated struct with a member of the same name (already `s32`).

### A third width change was tried and dropped

**Fact.** Widening `display_data::screen_shake_intensity` from `u32` to `s32`
made no difference (`ov29_022E34C8` scored 815 either way), so the tree is
unchanged there. Recorded so nobody re-runs it.

---

# 3. Shared decision: `DUNGEON_PTR` now has three declaration forms in one tree

| file | form, as landed | why |
|---|---|---|
| `src/dg_camera.c` | `extern struct dungeon *DUNGEON_PTR[2];` (was `[]`) | `ov29_022E34C8`: 815 → **0** |
| `src/dungeon_map_access_1.c` | `extern struct dungeon *DUNGEON_PTR;` (was `[]`) | the two functions joining it were written against the scalar |
| `src/overlay_29_02338350.c` | `extern struct dungeon *DUNGEON_PTR;` (new — the file declared none) | `ov29_0233804C` **requires** it |
| `src/dg_effect.c`, `src/overlay_29_0230F810.c` | `extern struct dungeon *DUNGEON_PTR[];` | unchanged |

**This is not taste, and it must not be normalised.** *Fact*, in both
directions: `ov29_022E34C8` does **not** match under `[]`, and `ov29_0233804C`
does **not** match under `[]` — the array form hoists the global load out of its
loop, leaving it one instruction short and using one extra callee-saved
register (`pop {r4, r5, r6, pc}` where the target has `pop {r3, r4, r5, pc}`).
Which spelling a file wants is decided per function by what the allocator does
with it.

### The measured axis is complete-vs-incomplete type, **not** array-vs-scalar

Full sweep in `wip/overlay_29_022E335C_all/cand/ptr-form.md`; **fact**, every
cell a real compile, body held byte-identical throughout:

| declaration | access spelling | score |
|---|---|---|
| `*DUNGEON_PTR[];` | `DUNGEON_PTR[0]->` / `(*DUNGEON_PTR)->` / `(*(DUNGEON_PTR + 0))->` | **815** |
| `*DUNGEON_PTR;` | `DUNGEON_PTR->` / `(&DUNGEON_PTR)[0]->` / `(*&DUNGEON_PTR)->` | **0** |
| `*DUNGEON_PTR[1];` `[2]` `[3]` `[4]` `[8];` | `DUNGEON_PTR[0]->` | **0** |
| `*const DUNGEON_PTR[];` / `*const DUNGEON_PTR;` | either | 875 |

**Fact:** the access spelling is not the lever — three spellings of the same
lvalue are all 815 under `[]` and all 0 under `[2]`. **Fact:** the element count
is not the lever either — `[8]` is as good as `[1]`. **Inference** (stated as
such in that pass): what moves mwcc is the *completeness* of the declared type;
an incomplete `extern T *G[];` keeps a store-to-load forward and the
`DUNGEON_PTR[0]` load CSE alive together, and completing the type breaks both.

This **refines** `docs/MATCHING_TIPS.md`'s existing entry, which frames the axis
as scalar-vs-array; scalar merely happens to sit on the complete side.

### Why `[2]` for `src/dg_camera.c`

* **Fact:** `[]` and `[2]` are compatible types, so **not one use site moves** —
  13 occurrences of `DUNGEON_PTR[0]` in the landed file are untouched (counted
  in the tree).
* **Fact:** in-tree precedent predates this pass — `src/special_move_types.c:17`
  already declares `extern struct dungeon *DUNGEON_PTR[2];`, landed in pmd-sky
  `964beafb` for `ov29_0231AFB4` / `ov29_0231B008` (460 and 515 → 0).
* **Fact:** `DUNGEON_PTR` measures **8 bytes** — in
  `asm/overlay_29_data_023534E0.s` the label is followed by two `.byte`
  quadruples before the next `.global`. So `[2]` is the symbol's exact extent.
* **Fact:** the scalar form also reaches 0 on all ten functions of that group,
  at the cost of rewriting all 13 use sites. Either is available.
* **Fact:** the file's three pre-existing bodies were proven byte-neutral the
  target-free way — same source under both declarations, **31 instructions
  identical** — with the comparator self-checked first against `ov29_022E34C8`,
  which it correctly reported as DIFFERENT.
* **Fact:** no header in `include/` declares `DUNGEON_PTR` at all, so the change
  cannot reach another translation unit.

### Why the scalar for `src/dungeon_map_access_1.c`

Two use sites were rewritten `DUNGEON_PTR[0]->` → `DUNGEON_PTR->`
(`IsTileGround`, `IsWaterTileset`). **Fact:** that rewrite was measured
byte-identical (`probe/m.c`) *before* it was made, and the three builds confirm
it.

**Open question, and it is genuinely unknowable from bytes:** whether retail
wrote `[2]`, `[]`, the scalar, or something else. Six declarations reach score
0. `[2]` was chosen because it is in-tree, exactly sized and type-compatible —
that is the whole case, and it is offered as such, not as a discovery.

---

# 4. The declaration cleanup — and the hole in the method that found it

Call-site-derived `extern`s scattered in other `.c` files were replaced by an
`#include` of the header that now owns each function.

**Fact, counted from the diff**: 21 removed declaration lines in `src/`; drop
the 2 `DUNGEON_PTR` re-spellings covered above and 19 lines remain, which are
**18 distinct function declarations across 14 files** —
`src/overlay_29_022E4338.c` carried two identical copies of one declaration.
Of those 18, **16 in 12 files** were replaced by
*adding* an include — the commit message's count. The remaining two sat in files
that already included the owning header: `src/overlay_29_022E3F20.c` (already
had `dg_effect.h`) and `src/overlay_29_02338350.c` (same translation unit as the
new definition).

### Eight of them were wrong, and a build could never have said so

**Fact.** Eight declarations of `PlayEffectAnimationEntity`, across seven files,
all with at least one wrong parameter type and **seven with the wrong return
type**:

```c
/* seven files declared this */
extern void PlayEffectAnimationEntity(struct entity *entity, s32 id, s32 a, u8 b,
        s32 c, s32 d, s32 e, s32 f);

/* src/overlay_29_02308FBC.c:120 — return type right, tail still wrong */
extern s32 PlayEffectAnimationEntity(..., u8 b, s32 c, s32 d, s32 e, s32 f);

/* what the function's own prologue says, and what include/dg_effect.h now carries */
s32 PlayEffectAnimationEntity(struct entity *entity, s32 id, s32 param_3, u8 param_4,
                              s32 param_5, u8 param_6, s16 param_7,
                              struct unk_0201C000 *param_8);
```

Per-parameter evidence, all **fact**, read off this function's own prologue
(stack args begin at `sp+0x48` = 6 saved registers + `0x30`):

| parameter | declared | actual | instruction |
|---|---|---|---|
| `param_6` | `s32 d` | `u8` | `ldrb r0, [sp, #0x4c]` |
| `param_7` | `s32 e` | `s16` | `ldrsh r0, [sp, #0x50]` |
| `param_8` | `s32 f` | `struct unk_0201C000 *` | `ldr r3, [sp, #0x54]`, then `ldrh r0, [r3], #2` six times |
| return | `void` (×7) | `s32` | `mvn r0, #0` on every early exit; `src/overlay_29_02308FBC.c:789` compares the result against `-1` |

**Byte-neutral, predicted then confirmed.** Every affected call site passes
literal `0`/`1`/`2`/`-1` for the changed parameters and a literal `0` for the
pointer — an `s32` and a `u8`/`s16`/pointer argument spelled as the same literal
are passed identically — and the return value is discarded everywhere except
`src/overlay_29_02308FBC.c:789`, which already stored it into an `s16`. *Fact*:
all three builds are OK with all eight deleted.

### The methodological finding — record this, it is the transferable part

**A line-oriented grep for a declaration cannot see one that wraps across two
lines, and the first census run here missed all eight because of it.** Every one
of those declarations puts the name and `(` on one line and the `);` on the
next, so a `NAME(...);` pattern matches none of them and the census reports a
clean *"no declarations found"* for a function that had eight.

Why that matters more than it looks: CLAUDE.md's own landing checklist says a
**build cannot catch a stale declaration** — two declarations in two translation
units never meet, so the ROM still matches with a wrong prototype sitting in the
tree. **The grep is the only detector**, and a detector with a blind spot
returns a false negative that nothing downstream contradicts. These eight were
caught only because `wip/overlay_29_022E335C_all/STATUS.md` listed them **by
hand**.

**What to run instead:** a multiline-aware search, e.g.
`rg -U --multiline-dotall '\bNAME\s*\([^;{]*\)\s*;' src/ include/`, and *read*
the hits rather than counting them. **Inference, not measured:** no pattern is
proof against every spelling (a macro, a `typedef`'d signature, a name split by
a line continuation), so treat the grep as a lead generator and keep the
hand-listing habit that actually worked.

The seven-declaration census in the 0230F02C group used the same line-oriented
method. Those seven are all single-line, so that list is right — but it was
verified, not assumed, after this was found.

### One declaration disagreement flagged rather than reconciled

**Fact.** `AnimationDelayOrSomething`'s parameter is `bool8` in the new
`src/dg_effect.c` declaration and `s32` at `src/overlay_29_022F0590.c:105`. The
call site here emits `and r0, r0, #0xff` on the `param_5 == 2` result, which an
`s32` parameter would not produce. That file passes a literal `1`, so it is
unaffected — but the disagreement is real and is left visible rather than
silently normalised.

---

# `AuraBowIsActive` — it needed no change in the end, and one cast it did need

**Fact.** The body is unchanged from the 200-scoring candidate. What moved was
the tree: `ITEM_INVALID = -1` landed separately in `967fe53b`
(`include/item.h:9`), which made `enum item_id` signed under `-enum min`, and
the target's `ldrsh` fell out on its own. Measured against a context generated
from the tree as it stands: **score 0 in NORTH_AMERICA, EUROPE and JAPAN with
nothing modified.**

**Fact, the one thing that was not anticipated.** With the enum signed, the
argument at the `HasHeldItem` call is a `short` → `enum item_id` conversion, and
`-W error` rejects it. The scratch does not carry the build's `-W` flags, so
this only appeared at landing. The landed body therefore reads:

```c
return HasHeldItem(entity, (enum item_id)monster->held_item.id);
```

(`src/overlay_29_0230F810.c:268`.) **That cast is there for the diagnostic, not
the codegen** — the codegen was already right once the enumerator existed. It is
easy to confuse with a different measurement in the same wip: a `(s32)` cast at
that call site was measured **not** to fix the `ldrh`/`ldrsh` row.

### The untested alternative — do not read it as a recommendation

**Declaring `struct item::id` as `enum item_id` rather than `s16`** would remove
the conversion and the cast with it. **It was NOT measured**, and it is a
shared-type change with a wide blast radius:

* `struct item` is used everywhere items are; every read of `.id` feeding a
  signed context, and every existing `*(s16 *)&…` cast around item ids in
  `src/`, is a site that could move.
* `struct item_volatile` is a **parallel** declaration of the same layout
  (`volatile s16 id;`) that exists to match `AiDecideUseItem`. It would have to
  change too, or the two disagree.
* **Fact:** `-enum min` sizes `enum item_id` from its enumerator range
  (−1 … 1400), which still fits two bytes — so the *width* is not the risk; the
  **signedness of every load** is.

Recorded because it is the obvious next question and nobody has answered it.

### A trap worth carrying forward

`AuraBowIsActive` measured 200 for the whole of that group's work **purely
because the wip's generated context predated `967fe53b`**. An entire
shared-enum decision section was written on that basis and is now obsolete.
**Regenerate the context before theorising about the C** — a surprising score is
more often a stale header set than a wrong candidate.

---

# `TryNonLeaderItemPickUp` — 150 → 0

**Fact**, applied cumulatively, each row a real compile
(`wip/overlay_29_0230F02C_all/cand/census.md` §2):

| # | change | NA score |
|---|---|---|
| — | the body as it stood | 150 |
| 1 | `s32 i;` → **`int i;`** | **130** |
| 2 | name the `item->id` CSE (`s32 item_id;` declared **immediately before `i`**) | **100** |
| 3 | the bag fill loop counts with **`best`**, not `count` | **50** |
| 4 | declare **`best` before `bag`** | **0** |

**Fact:** no step changed the instruction count (`rows base 316 cur 316 (+0)`
throughout), the final body is `STRUCT 0 + COLOUR 0`, and it scores 0 in all
three regions. **Fact:** nothing under `pmd-sky/` was modified to get there — no
shared type, no header, no enum — and no `volatile`, `asm`, intrinsic, pragma or
dead statement.

**Ablation** (§4) — every ingredient is load-bearing, each row the final body
with exactly one thing reverted:

| reverted | score | what breaks |
|---|---|---|
| (control) | **0** | — |
| `int i` → `s32 i` | 50 | `bag_enabled` ↔ merge index, 9 rows |
| named `item_id` → the bare CSE | 30 | `item_id` ↔ `bag_enabled`, 6 rows |
| fill loop counts with `count` again | 100 | `count` ↔ `best`, 17 rows |
| `bag` declared before `best` | 50 | fill counter ↔ `bag`, 7 rows |
| drop the `((const struct item **)slots)[i]` cast | 815 | **17 structural** |
| `slots` declared before `slot_ids` | 1856 | 11 structural — the stack layout flips |

The last two are pre-existing findings from earlier in that group's work; they
were not disturbed and are still required.

**The mechanism offered for it is INFERENCE, and that pass says so itself.** The
reading is that a local with two **disjoint webs** does not give both webs its
declaration index — the second is coloured as if it were an unnamed temp and
loses to any declared local it competes with, from *every* declaration position,
which is why three exhaustive order sweeps could not reach it. `count` had
exactly that shape. It is consistent with every measurement above; it is not
independently proven against compiler internals.

**The methodological finding is the durable part.** The in-tree source census
produced nothing; the `MATCHING_TIPS.md` / `COMPILER_INTROSPECTION.md` census
produced all four changes. And change 2 had been tried before and written down
as a true negative — *"a named `s32 item_id = item->id;`, swept over 5 positions
→ 150, no change"*. That was **true at the 150 base**, and worth 30 at the 130
base. **A negative result from a sweep is a fact about the base it was measured
on, not about the technique.** Both that and the `const`-on-the-array-element
trick were appended to `docs/MATCHING_TIPS.md` (workspace repo, not this
commit).

---

# Regions

**Fact**, asserted per run by each group's `mktarget.py`, which resolves the
target text per region and enumerates every delta:

| group | EUROPE | JAPAN | `#if` needed in the C |
|---|---|---|---|
| 02337EC0 | **byte-identical** to NA | 5 lines, all header-driven struct offsets | **none** |
| 022E335C | **byte-identical** to NA | 17 lines: 16 field displacements + 1 message id | one `#ifdef JAPAN` for `REVEAL_WHOLE_FLOOR_MESSAGE` (`0x888` / `0xB77`) |
| 0230F02C | **byte-identical** to NA | 19 lines: 4 field offsets + 6 message ids | six `MESSAGE_C5E`…`C63` macros in one `#ifdef JAPAN` block |

Every message-id literal was **scraped by the tooling out of that region's own
resolved pool words / immediates**, not typed by hand — `REVEAL_WHOLE_FLOOR_MESSAGE`
from `_022E34AC`, and the six ids cross-checked against the
`TRY_NON_LEADER_ITEM_PICK_UP_OFFSET = -0x2C1` the `.s` itself spells. The macro
names carry the NA literal and nothing else, deliberately, since the message
text is unknown.

**Scratch-level region coverage is uneven, and this is the honest record:**

* 02337EC0 — all **6** functions scored **0 in NA + EU + JP** against
  region-generated contexts.
* 0230F02C — all **10** scored **0 in NA + EU + JP**.
* 022E335C — **NORTH_AMERICA only**. No EU or JP *context* was ever generated
  for that group. Its EU/JP case rests on the target-level assertions above
  (EU byte-identical; every JP delta header-driven) **plus the three real
  builds** — not on a score.

The three matching builds are what actually settle all three regions. The
scratch scores are the stronger evidence only where they exist.

---

# Naming

Everything stayed at placeholders except where the tree already had a name.
Per CLAUDE.md, decompiling is not a licence to name.

| new type | named for | why that name |
|---|---|---|
| `struct unk_02337EE8` | the function `ov29_02337EE8` | the member it types has no global of its own — it is a member of `struct dungeon`. Two functions force the grouping; the lower address wins. Member spelled `field_0x286b0`, its offset. Follows the tree's own precedent for the only other grouped `struct dungeon` member, `struct unk_022FBD24 field_0x3dcc` |
| `struct unk_0201C000` | `InitOamAdjustmentInfo` (`0x0201C000`) | the function that takes it |
| `struct unk_022BF274` | `ov10_022BF274` | the function that takes it |
| `struct unk_0235354C` | the **global** `ov29_0235354C` (`0x0235354C`) | a global beats a function, per the collision rule |

**None of these is ported from pmdsky-debug.** Upstream was read for layout
only.

Two size facts behind those types:

* **`struct unk_022BF274` is 0x2C bytes, not 0x28** — three independent frames
  say so: `PlayEffectAnimationPixelPos` (`sub sp, #0x2c`, struct at `sp+0`),
  `ov29_022E6A00` (`sub sp, #0x38` = a 12-byte scratch plus the struct at
  `sp+0xC`), and `PlayEffectAnimationEntity` itself (`sub sp, #0x30` = a
  `struct position` at `sp+0` plus the struct at `sp+4`). At 0x28 this
  function's frame came out `sub sp, #0x2c` with `r3` pushed for alignment.
* **`ov29_0235354C` is 8 bytes**: `asm/overlay_29_data_023534E0.s` puts the next
  symbol (`TOP_SCREEN_STATUS_PTR`) at `0x02353554`, and `ov29_022E6A00` stores a
  `MemAlloc` result into `[ov29_0235354C + 4]` and dereferences it. Only offset
  0 is used by the two functions here, so `extern u8 ov29_0235354C;` would also
  compile and also match — it would simply be the wrong type for the data.

`UseThrowableItem`'s prototype in `src/overlay_29_0230F810.c` is explicitly a
**call-site-derived guess** (every call site sets only `r0` and reads no result;
its body treats `r0` as `struct entity *`). When it lands, move the prototype
into its own header and delete that line.

---

# Open questions for a reviewer

* **Is `struct unk_02337EE8` the right shape to carry upstream?** It is a
  deviation from pmdsky-debug, whose `struct dungeon` is flat across this range.
  The offsets agree; the grouping does not exist there.
* **Where does the sub-object end?** Its start at 0x286B0 is forced by
  measurement; the end is a judgement call, and there is no second consumer
  anywhere in the ROM (`0x42B4` appears nowhere else) to corroborate it.
* **LEDGER item 10 vs the census result.** A structurally similar probe folded
  where the real function does not; the two measurements differ in two ways at
  once and were never separated.
* **Whether retail wrote `extern struct dungeon *DUNGEON_PTR[2];`** — six
  declarations reach score 0 and the bytes cannot distinguish them. The tree now
  deliberately carries three different spellings across five files; none may be
  normalised without re-measuring the functions that forced each.
* **The two lost/stale comments at `dungeon` offset 0x1E–0x1F.** *Partly resolved
  by the 2026-09-19 addendum above.* The deleted Speed Boost description turns out
  to have documented a real mechanic that lives on `monster + 0x11F`, not at
  `dungeon` 0x1F, so nothing true about 0x1F was lost — and the point is worth
  raising upstream rather than only recording here. What is still open is the
  **surviving** comment, which contradicts the `s16` beneath it, and the member's
  **name**, which remains upstream's guess. Both need a human with full context.
* **Typing `struct item::id` as `enum item_id`** would delete the
  `(enum item_id)` cast in `AuraBowIsActive`. Untested, wide blast radius,
  `struct item_volatile` would have to move with it.
* **`AnimationDelayOrSomething`'s parameter** is `bool8` here and `s32` at
  `src/overlay_29_022F0590.c:105`. The asm supports `bool8`; the other site is
  unaffected but the tree now contradicts itself.
* **`BAG_ITEMS_PTR_MIRROR` is declared `u8 *` at `src/main_0200CA54.c:8`** and
  `struct bag_items *` elsewhere. Not touched here — different translation unit,
  out of scope — but it is the same class of error as the eight above.
* **The declaration census across the whole tree has never been re-run
  multiline-aware.** These eight were found by hand. There is no reason to think
  they are the only wrapped declarations with wrong types, and a build will
  never report one.
* **`ov29_022E34C8`'s mechanism** (incomplete array type keeping two
  optimisations alive) is inference from an exhaustive score sweep, not from
  compiler internals.
* **`TryNonLeaderItemPickUp`'s split-web mechanism** is likewise inference; the
  four changes and their scores are facts, the explanation is not.
* **All 26 functions keep `sub_`/`ov29_` placeholder names.** This was
  decompilation, not identification; several of them (`AuraBowIsActive`,
  `TeamMemberHasItemActive`, `RevealWholeFloor`, …) carry names that were
  already in the tree, and the rest are deliberately unnamed.
