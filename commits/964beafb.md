# `964beafb` — Decompile 38 more callees; clear five more asm files entirely

| | |
|---|---|
| **Commit** | `964beafb` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of [`9b3650fe`](9b3650fe.md) |
| **Verified** | all three ROMs: `pmdsky.us.nds: OK`, `pmdsky.eu.nds: OK`, `pmdsky.jp.nds: OK` |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The PR diff and the matching build are the sources
> of truth — see [the README](../README.md). Claims are labelled **fact** (read
> off the asm/data, or from an existing in-tree header) or **inference**.

---

Thirty-eight functions in **nine groups**, each group chosen so that it consumes
either a whole `.s` file or its head — so `extract_function.py` only ever
*merges* into the preceding `src/` object and nothing splits. **Five files are
consumed entirely**, which is the headline: `main.lsf` loses five asm objects
outright and renames four more.

This is a batch commit, not a single decompilation, so the note is organised by
*what a reviewer has to check* rather than function by function. The three items
worth reading before the diff are the **shared-type change to `dungeon.h`**, the
**three coexisting spellings of `DUNGEON_PTR`**, and the **nineteen call-site
declarations removed**.

## The nine groups

| asm file | functions | landing | regions measured |
|---|---|---|---|
| `asm/overlay_29_0231AFB4.s` | `ov29_0231AFB4`, `ov29_0231B008`, `ActivateMotorDrive`, `TryActivateFrisk` | whole file → `src/special_move_types.c` | NA + EU + JP |
| `asm/overlay_29_022FBC4C.s` | `CountActiveMonsters`, `ov29_022FBC94`, `ov29_022FBD08`, `ov29_022FBD24`, `ov29_022FBD80` | whole file → `src/overlay_29_022FBBEC.c` | NA + EU + JP |
| `asm/overlay_29_02302388.s` | `ov29_02302388`, `UpdateStateFlags`, `IsProtectedFromNegativeStatus`, `ov29_023024E0`, `AddExpSpecial` | whole file → `src/dungeon_logic_2.c` | NA + EU + JP |
| `asm/main_02051098.s` | `sub_02051098`, `sub_020510C0`, `MtInit`, `MtNext` | whole file → `src/number_util.c` | NA only |
| `asm/main_02055528.s` | `IsMainCharacter`, `GetTeamMember`, `GetRecruitMentryIdBySpecies` | whole file → `src/main_02055410.c` | NA only |
| `asm/overlay_29_023350FC.s` | `ov29_023350FC`, `TryWeatherFormChange` | head → `src/overlay_29_023350D8.c` | NA only |
| `asm/main_020562B8.s` | `sub_020562B8`, `SetActiveTeam`, `sub_02056318`, `sub_0205633C`, `sub_02056360`, `GetActiveTeamMember` | head → `src/main_02056294.c` | NA only |
| `asm/overlay_29_022F0654.s` | `MakeTargetFaceUserAndIdle`, `ov29_022F067C`, `ov29_022F0780` | head → `src/overlay_29_022F0590.c` | NA + EU + JP |
| `asm/main_02013C30.s` | `GetMovesetIdx__02013CAC`, `IsReflectedByMagicCoat`, `CanBeSnatched`, `FailsWhileMuzzled`, `IsSoundMove`, `IsRecoilMove` | head → `src/moves_3.c` | NA only |

**Fact**, read off `main.lsf`: five `Object asm/…o` lines are deleted
(`main_02051098`, `main_02055528`, `overlay_29_022FBC4C`,
`overlay_29_02302388`, `overlay_29_0231AFB4`) and four are renamed to the
surviving tail (`main_02013C30`→`main_02013E54`,
`main_020562B8`→`main_020563BC`, `overlay_29_022F0654`→`overlay_29_022F07BC`,
`overlay_29_023350FC`→`overlay_29_0233544C`). 13 changed lines, which is exactly
5 deletions + 4 rename pairs.

**One landing-mechanics note worth carrying forward.** `asm/main_020563BC.s`
appears in the diff as a **new file**, not a rename — too much of
`asm/main_020562B8.s` was consumed for git to pair them. New `.s`/`.inc` files
are untracked, and `precommit.py --commit` only does `git add -u`, so this
commit needed `--add-untracked` (or an explicit `git add`) or it would have
referenced an object it did not contain. The same hazard applies to the other
three head-clears.

### "Regions measured" is not the same as "regions verified"

Four groups were measured on the NORTH_AMERICA context only. The argument in
each case (**fact**, from the group's `tools/mktarget.py`) is that the `.s`
block carries **no `#ifdef` at all**, and the tool *asserts* — rather than
assumes — that the EUROPE and JAPAN resolutions of every block are
byte-identical strings. The step from there to "so the EU/JP objects are also
right" is **inference**: the *context* differs per region even when the target
does not, because `struct monster` and `struct dungeon` carry `#ifndef JAPAN`
holes. That inference was closed after the fact by the commit's own EU and JP
ROM builds, which is the only reason it is safe to read past.

## Where the evidence lives

Each group has a harness under `wip/` in the workspace repo (not in pmd-sky, not
in this repo): `overlay_29_0231AFB4_all`, `overlay_29_022FBC4C_all`,
`overlay_29_02302388_all`, `main_02051098_all`, `main_02055528_all`,
`overlay_29_023350FC_head`, `main_020562B8_head`, `overlay_29_022F0654_head`,
`main_02013C30_head`. Each holds the frozen pre-landing `.s` with its sha1, the
per-region resolved targets, the generated context, the candidate bodies, and a
`STATUS.md` landing checklist.

**Four of the nine have a `LEDGER.md`** — `overlay_29_0231AFB4_all`,
`overlay_29_022FBC4C_all`, `overlay_29_023350FC_head` and
`overlay_29_022F0654_head` — and those dead ends are real, measured, and
reproduced below. `overlay_29_02302388_all` has a `TECHNIQUES.md` instead. **The
remaining four groups have neither**, and their `STATUS.md` says so explicitly
("nothing was falsified"). For those, the intermediate attempts are **not
recorded**; read the silence as "not written down", never as "nothing was
tried".

Every group used `mwcc_30_137` with the preset-101 flag string, **never varied**
— each harness pins it in its own `tools/probe.py`. All bodies reached
`current_score: 0` against a **per-function** target; where a combined
multi-function target was also scored, the residual (40, 95, 90, 25, 10) is the
known decomp.me artefact — the candidate object restarts each symbol's listing
at 0 while a hand-assembled combined target is one continuous blob, so pool
annotations and inter-function branch displacements disagree by construction.
The per-function scores are the measurement.

---

## Shared-type change: `struct unk_022FBD24` in `include/dungeon.h`

**This is the one edit to a shared header, and a reviewer should weigh it
first.** The two `#ifndef JAPAN` members of `struct dungeon` at 0x3DCC and
0x3E1C are moved into a new top-level `struct unk_022FBD24` and `struct dungeon`
now has a single member `struct unk_022FBD24 field_0x3dcc;` in their place.

**The layout argument (fact):** `u32 monster_unique_id[20]` then
`u32 unique_id_index`, same order, same 4-byte alignment, 0x54 bytes, still
inside the same `#ifndef JAPAN`. Nothing before or after them moves. The
comments move **verbatim** — identical text on a `-` and a `+` line, which is
the mechanical exception the repo's no-comments rule allows; no comment was
authored.

**Why the grouping is needed (fact, codegen evidence from two functions):** both
`ov29_022FBD24` and `ov29_022FBD80` address the array *and* the count off **one**
materialised register —

```
add r0, r0, #0x1cc
add r3, r0, #0x3c00       ; r3 = dungeon + 0x3DCC
ldr r2, [r3, #0x50]       ; the count, addressed off the ARRAY base
str r1, [r3, r2, lsl #2]  ; the element, register-scaled, displacement 0
```

With the two fields as plain siblings MWCC never produces that. The ledger
records both phrasings that were tried and failed:

| phrasing (fields left as siblings) | `ov29_022FBD24` | `ov29_022FBD80` |
|---|---|---|
| `DUNGEON_PTR[0]->…` spelled out at every use | 480 | 370 |
| `u32 *ids = dungeon->monster_unique_id;` + `dungeon->unique_id_index` | 595 | 180 |

The first folds the index into the address (`str r2, [r0, #0xdcc]`) and never
materialises a base; the second materialises the base but reaches the count
through a *second* one. Grouping took `ov29_022FBD24` to **0** on the first try.

**What that establishes and what it does not.** It establishes (fact) that
retail's code addresses 0x3DCC..0x3E1C as **one object**. It establishes nothing
about what the object *is* — hence the `unk_<ADDR>` / `field_0x<off>`
placeholder names. The two inner member names are pre-existing tree names and
are kept verbatim.

**Blast radius (fact, by grep):** `monster_unique_id` and `unique_id_index` have
**no other reader or writer** anywhere in `src/` or `include/` — only their
declarations in `dungeon.h`. So the only file whose spelling changes is the new
`src/overlay_29_022FBBEC.c`. (`monster_unique_id_counter`, further down
`dungeon.h`, is a different field at a different offset and is untouched.)

**Untried, and still open if the nesting is rejected:** nothing in plain C was
found that addresses one sibling member off another sibling's materialised base.
The only same-header alternative noticed was spelling the count as `ids[20]` — an
out-of-bounds read of a 20-element array, which is not what retail was compiled
from and **was not measured**.

## A second new type, file-local rather than shared: `struct unk_022C4C6C`

`src/overlay_29_023350D8.c` gains a file-scope

```c
struct unk_022C4C6C {
    enum type_id field_0x0;
    u8 field_0x1;
    s16 field_0x2;
    s16 field_0x4;
};
extern struct unk_022C4C6C CASTFORM_WEATHER_ATTRIBUTE_TABLE[];
```

It is **not** in a header, so it is not a shared-type change — but it does
assert a layout, and the evidence divides cleanly.

**Facts.** `CASTFORM_WEATHER_ATTRIBUTE_TABLE` is at 0x022C4C6C in
`asm/overlay_10_rodata_022C490C.s:158`; the gap to the next unrelated `.global`
(`BAD_POISON_DAMAGE_TABLE`) is **48 bytes = 8 × 6**, counted off the `.byte`
rows. `TryWeatherFormChange` indexes it with `weather * 6` (`mov r1, #6` /
`mul`) and does a `ldrb` at **+0**. Decoding the 48 bytes as eight 6-byte rows
gives, per weather id 0..7:

| weather | `field_0x0` | `field_0x2` | `field_0x4` |
|---|---|---|---|
| CLEAR, SANDSTORM, CLOUDY, FOG | 1 | 379 | 979 |
| SUNNY | 2 | 381 | 981 |
| RAIN | 3 | 382 | 982 |
| HAIL, SNOW | 6 | 380 | 980 |

**Fact:** `TYPE_NORMAL = 1`, `TYPE_FIRE = 2`, `TYPE_WATER = 3`, `TYPE_ICE = 6`
(`include/enums.h`), and `_MONSTER_ID_GENDERED(CASTFORM_NORMAL, 379)` puts
Castform's base id at 379 with the gendered counterpart 600 higher — which is
exactly the `field_0x4 = field_0x2 + 600` the bytes show. So the decode is
self-consistent with the Forecast ability, and `enum type_id` for `field_0x0` is
corroborated by the data and not only by the assignment target. **Inference:**
that the row *means* "the type and sprite Castform takes in this weather".

**Three caveats a reviewer should carry.**

1. **The commit deviates from the harness here.** `wip/overlay_29_023350FC_head`
   measured `field_0x0` as **`u8`** — that is the spelling the score-0 scratches
   used. The landed file says `enum type_id`, with the commit message's
   justification "it feeds `monster->types[0]`". The byte-neutrality argument is
   sound (**fact**: `enum type_id` maxes at `TYPE_NEUTRAL = 18`, so `-enum min`
   sizes it to one byte), and the three matching ROM builds settle it. **Whether
   a scratch was re-run under the `enum` spelling is not recorded — I did not
   check, and this note does not assert it was.**
2. **The struct row overlaps three separate `.global` symbols.** `+2` is
   `ov10_022C4C6E` and `+4` is `ov10_022C4C70`, both declared in the data `.s`
   and both loaded independently by `asm/overlay_29_022F9194.s`. So the tree now
   describes the same bytes two ways: as an array of 6-byte rows here, and as
   three separate labels there. Neither is wrong; they will have to be
   reconciled when that asm file lands.
3. **`field_0x4` is inference, not fact** — nothing in this group reads it. It
   is taken from the decoded bytes alone.

The harness also records an **equally-matching alternative**, measured at score 0
the same day: drop the struct entirely and write
`extern u8 CASTFORM_WEATHER_ATTRIBUTE_TABLE[];` indexed
`[GetApparentWeather(entity) * 6]`. The struct form was chosen; that choice is
taste, not evidence, and swapping requires changing the extern and the index
together.

## `DUNGEON_PTR` now carries three spellings, deliberately

After this commit the tree declares one symbol three ways, in different
translation units:

| spelling | where | why |
|---|---|---|
| `extern struct dungeon *DUNGEON_PTR[2];` | `src/special_move_types.c` (**new**) | see below |
| `extern struct dungeon *DUNGEON_PTR;` | `src/overlay_29_023350D8.c` (new here; 10+ pre-existing files) | see below |
| `extern struct dungeon *DUNGEON_PTR[];` | `src/overlay_29_022FBBEC.c`, `src/dungeon_logic_2.c`, `src/overlay_29_022F0590.c` (the tree's usual form) | default |

**This is a codegen lever, not a style question**, and it is the single most
surprising thing in the commit.

**Measured (fact), all three regions, on the landed translation unit:** with the
*incomplete* array type `[]`, MWCC hoists the `ldr r0, [r1]` for `DUNGEON_PTR[0]`
out of the **outer** loop of `ov29_0231AFB4` and `ov29_0231B008`, where retail
keeps it at the top of the loop body. Scores **460** and **515**. Give the array
a size and it stops:

```
array_open  []   ov29_0231AFB4=460  ov29_0231B008=515
array_1     [1]  ov29_0231AFB4=0    ov29_0231B008=0
array_2     [2]  ov29_0231AFB4=0    ov29_0231B008=0
scalar           ov29_0231AFB4=0    ov29_0231B008=0
```

`(*DUNGEON_PTR)->…` scores the same as `DUNGEON_PTR[0]->…`, which is what shows
the lever is the **declaration**, not the access spelling. Fourteen structural
rows, and the loop shape was never the problem: block-local vs function-scope
temp, `&a[i*2]` vs `a + i*2`, `for` vs `do/while` and a 2-D cast all compiled
byte-identically, while the single-subscript `[i*2+j]` was *worse* (23 rows — it
folds `0x198E4` into the store displacement and loses the
`add lr, r0, ip, lsl #4` / `str r2, [lr, r4, lsl #3]` pair). A `volatile` cast
was used **purely as a diagnostic** to name the defect (score 130, all 14
structural rows gone at once) and then discarded — it is not an allowed fix.

`[2]` over `[1]` or the scalar on evidence, not preference: **fact**, the
symbol's extent is 8 bytes — `asm/overlay_29_data_023534E0.s:43` is
`.global DUNGEON_PTR` followed by two 4-byte `.byte` rows before the next
`.global` — and `src/overlay_29_022DEAB0.c` already reads and writes
`DUNGEON_PTR[1]`. `[]` and `[N]` are *compatible* types in C, so this **completes**
the tree's incomplete declaration rather than contradicting it, and no other file
needs to change.

The scalar spelling in `src/overlay_29_023350D8.c` is the same lever pointing the
other way: `ov29_023350FC` scored **930** with the array spelling (one row, the
same hoist out of a four-store loop) and **0** with the scalar. That spelling is
already the majority form in a dozen in-tree files, so nothing new is introduced
there.

**Inference, stated as such:** the mechanism is that with an incomplete array
type MWCC treats the element access as an ordinary global read it may hoist
across the loop's stores, and a complete object type makes it conservative. That
reading was **not** verified against the compiler's internals; what is verified
is the table.

**A reviewer may reasonably want one canonical spelling.** All three compile and
link to the same symbol, so this is not a correctness problem — but the tree now
encodes a codegen fact in a declaration, and that is invisible to anyone who
"tidies" it later.

## Nineteen call-site declarations removed — and a count to re-check

Nineteen declarations of the newly-landed functions, written from call sites by
earlier contributors, are deleted and replaced by an `#include` of the owning
header, across thirteen files. **The commit message says "Eighteen".** I count
nineteen `-` lines in the diff (one of them,
`src/overlay_11_02307334.c:116`, held two run-together externs and was rewritten
rather than deleted, which may be where the discrepancy comes from). The code is
the same either way; only the message's arithmetic is at issue.

| file | declaration removed | agrees with the landed signature? |
|---|---|---|
| `src/dungeon_ai_itcm.c` | `extern void ov29_0231B008();` | K&R, but compatible |
| `src/dungeon_ai_targeting.c` | `UpdateStateFlags` | yes |
| `src/dungeon_recruitment.c` | `GetActiveTeamMember` | yes |
| `src/main_0205A288.c` | `GetTeamMember` | yes |
| `src/main_0205B008.c` | `GetActiveTeamMember` | yes |
| `src/main_0205D11C.c` | `extern s32 MtNext(void);` | **no — `s32` vs `u32`** |
| `src/move_orb_effects.c` | `IsProtectedFromNegativeStatus` | yes |
| `src/overlay_11_02307334.c` | `GetActiveTeamMember`, `GetTeamMember` | yes |
| `src/overlay_29_023000E4.c` | `…GetActiveTeamMember(int roster_idx);` | **no — `int` vs `s32`** |
| `src/overlay_29_02308FBC.c` | `ActivateMotorDrive`, `AddExpSpecial`, `GetActiveTeamMember` | yes |
| `src/overlay_29_02308FBC.c` | `extern int ov29_022F0780();` | **no — `int` vs `void`** |
| `src/overlay_29_02308FBC.c` | `extern int ov29_022FBD24();` | **no — `int` vs `bool8`** |
| `src/overlay_29_0230BBAC.c` | `extern int IsRecoilMove(enum move_id);` | **no — `int` vs `bool8`** |
| `src/overlay_29_0230BBAC.c` | `UpdateStateFlags` | yes |
| `src/overlay_29_02311010.c` | `TryWeatherFormChange` | yes |
| `src/type_effectiveness.c` | `UpdateStateFlags` | yes |

**A second count to re-check.** The commit message says "Three were required
rather than cosmetic". By the criterion *"a compiler would reject this line once
this commit's include lands in the same translation unit"*, I count **five** —
the five bolded rows above. `int` and `s32` (`signed long`) are distinct C
types; so are `s32` and `u32`; so are `int` and `void`/`bool8`. I could not
recover the criterion behind "three", so I am recording the discrepancy rather
than guessing which of us is using the wrong one.

**The important property of all nineteen — and the reason the list matters at
all — is that until this commit none of them could be caught by a build.** Each
lived in a different translation unit from the definition, so the two
declarations never met and the ROM matched regardless. Only a grep finds them.
Two of them (`MtNext` `s32`→`u32`, `IsRecoilMove` `int`→`bool8`) also changed a
call site's *declared* signedness or width:

- `main_0205D11C.c:292` is `mission->description_id = MtNext() & 0xFFFFFF;`,
  and `description_id` is `s32` (**fact**, `include/mission.h:190`). The mask
  forces the value non-negative, so `(s32 & 0xFFFFFF)` and `(u32 & 0xFFFFFF)`
  produce the same bits and the same `str`.
- `overlay_29_0230BBAC.c:156` is
  `AbilityIsActiveVeneer(…) && IsRecoilMove(move_id)`. Compiling that exact
  shape with the callee declared `int` and then `bool8` was **measured** to
  produce byte-identical output (25 identical instructions, both variants) —
  `&&` tests with `cmp r0, #0` after the `bl`, with no `and r0, r0, #0xff`
  truncation. The line two below already calls a landed `bool8` function in the
  same position, which is independent corroboration.

Both are confirmed by the three matching builds, which is what actually settles
them.

**One stale declaration was deliberately left alone.**
`src/overlay_29_022F0EDC.c:76,78` still carry
`extern s32 TryPointCameraToMonster();` and
`extern s32 WaitUntilAlertBoxPauseIsOver();` in K&R form. That file does not
include `overlay_29_022F0590.h`, and this commit puts `TryPointCameraToMonster`
in a file-scope `extern` in `src/overlay_29_022F0590.c` rather than a header, so
the two never meet. The empty argument lists are weaker than the real
signatures; fixing them is a separate change. **Flagged, not fixed.**

## Three functions that do not exist in the JAPAN build

**Fact:** `ov29_022FBD08`, `ov29_022FBD24` and `ov29_022FBD80` sit inside one
`#ifndef JAPAN` in `asm/overlay_29_022FBC4C.s` that wraps their
`arm_func_start`/`arm_func_end` pairs. The harness *asserts* that the JAPAN
resolution of the whole file contains exactly `['CountActiveMonsters',
'ov29_022FBC94']` and refuses to invent a JP target for the other three. So
their definitions in `src/overlay_29_022FBBEC.c` and their prototypes in
`include/overlay_29_022FBBEC.h` both carry `#ifndef JAPAN`, following the
existing in-tree idiom (`src/overlay_29_022FB538.c` + its header). They **must**
be guarded: their bodies read the dungeon members at 0x3DCC/0x3E1C, which
`dungeon.h` itself wraps in `#ifndef JAPAN`.

The single call site, `src/overlay_29_02308FBC.c:1161`, was already inside its
own `#ifndef JAPAN`; no change was needed there.

## What each group's match actually turned on

These are the measured levers, from the four ledgers. They are recorded here
because the shapes look arbitrary in the diff and a reviewer will otherwise be
tempted to "tidy" them.

**`ov29_022F067C` — five dead stores that emit nothing.** The body zeroes every
field of a local `struct move` and then fills every field. **Fact:** those five
stores are dead-store-eliminated and cost no instruction, but removing them
leaves the function at score 20 with `mov r2, #0` and `mov r4, #1` transposed
and everything else exact. **Inference:** MWCC numbers the constant webs *before*
DSE runs, so the dead stores are what make the `0` web older than the `1` web.
Thirty-odd variants were falsified around this — field order permutations (60,
218, 32, 80, 120), comma expressions, aggregate initialisers (1405/1815 — MWCC
lowers them to a pool copy loop), named `zero`/`one` locals (all inert, constant
propagation folds them back), and **dead stores to plain locals, which do
nothing** (`n = 0;` and `i = 0;` both stayed at 20). The distinction between a
dead store to a *struct field* and one to a *local* is the sharp part.

**`TryWeatherFormChange` — a `const` view that exists only to split a CSE.**
**Fact:** retail reads `monster->apparent_id` **twice** at each of two sites
(`ldrsh r0, [r7, #4]` / `cmp` / `beq` / `ldrsh r0, [r7, #4]` / `bl`), with no
intervening call, store or redefinition. Every ordinary spelling CSEs them into
one load — the whole residual, 275. The landed C declares
`const struct monster *cmonster = monster;` and reads the compare through it:
`const` changes the type, and therefore the CSE key, without changing a byte of
address arithmetic.

**A reviewer must be told what this does and does not establish.** The bytes
establish only that **one** of the two reads sits in a different CSE class from
the other. *Which* read, and whether via a `const` local, a `const` cast, or
`volatile`, is **undecidable from the bytes** — the ledger records four
score-0 spellings. `const` was chosen because it is plain C with no cast and
asserts the least. **Do not read it as recovered source.** The falsified list is
long and worth not re-walking: identity pointer casts, byte-offset puns,
`monster[0].apparent_id`, pointer/alias locals (copy propagation folds them, and
any extra local that does not coalesce costs 39 colour rows), reading through
`entity->info` (285 — it *does* defeat the CSE but pays an extra `ldr`), and six
control-flow shapes. **Still untried:** a union or second struct declaration over
offset 4 giving the two reads genuinely different member types; a different
prototype for `DungeonGetSpriteIndex` in this TU; and whether the same `const`
split appears at the other `bl DungeonGetSpriteIndex` sites still in asm
(`asm/overlay_29_022E1AD4.s`, `022FC9C0.s`, `023026FC.s`) — **that last check is
the one that would tell a reviewer whether this is recovered source or a
stand-in, and it was not done.**

**`ov29_023024E0` — predecessor count decides if-conversion.** **Inference,
measured:** MWCC if-converts a `return CONST;` with exactly **one** predecessor;
a tail with two or more stays a real block. Three separate early returns emitted
three if-converted exits (6 rows off); folding them into *one* `return TRUE`
reached by `||` and *one* `return FALSE` reached by an enclosing `if` took it to
0. `AddExpSpecial` needed the sibling trick: `cond ? FALSE : f(...)` rather than
`!cond && f(...)`, because `?:` has a value and must materialise the `movne
r0, #0` that both paths then test once.

**`MtInit` — the loop counter *is* the global.** **Fact:** the target's exit
stores the loop variable, not the constant `#0x270`, and a local counter left
one row (a missing `b` to the bottom test). Making `MT_TABLE` itself the counter
took it to 0. **Inference:** MWCC rotates a counted loop before register-promoting
a global, so a global counter keeps the top-tested shape and *then* collapses
into `ip`. `MtNext` in the same file needed the opposite shape
(`for (kk = 0; kk < 227; kk++)` rotated, `for (; kk < 623; kk++)` not) — both
forms are load-bearing.

**`sub_02056360` — `bool32`, not `bool8`.** `bool8` emits a trailing
`and r0, r0, #0xff` the target does not have (score 105). Note the contrast
inside the same group: `sub_020562B8`'s target *does* carry that mask, so it
really is `bool8`.

**`IsMainCharacter` — a negated chain in an early return.** `idx == 2 || idx == 3
|| idx == 4` folds to a range test (`sub`/`cmp`/`bhi`); the negated
`idx != 2 && idx != 3 && idx != 4` hoisted to an early `return 0` reproduces the
target's `cmp`/`cmpne`/`cmpne`/`beq` chain without the double negation the
existing MATCHING_TIPS entry prescribes.

**`ov29_0231AFB4` / `ov29_0231B008` — `s32 j;` declared inside the outer loop
and after the pointer local.** Six placements were measured; only that one is 0
(declaring `j` *before* `slots` in the same block scores 130). Same rule, other
group: in `ov29_022FBD80`, `s32 i;` must be declared *before*
`s32 n = p->unique_id_index;`. **Inference, already tree folklore and confirmed
again twice here:** MWCC hands out `r0..r3` then `ip` in **declaration order**.

## Naming

**Nothing was named or renamed.** Every `sub_<addr>` and `ov29_<addr>` keeps its
name; every function that already carried a real name in the `.s`
(`ActivateMotorDrive`, `TryWeatherFormChange`, `UpdateStateFlags`,
`AddExpSpecial`, `IsMainCharacter`, `MtInit`/`MtNext`, the six move predicates,
`GetMovesetIdx__02013CAC` with its double underscore) keeps it verbatim. No data
symbol was relabelled — `MT_TABLE` keeps the name the tree already gives it, and
`_020AFF80` / `_020AFF88` keep their `_0<addr>` placeholders. The two new types
are placeholders per the repo's rules: `struct unk_022FBD24` named for the
function that exercises the whole object, `struct unk_022C4C6C` for the global's
address.

**Inference offered for a future naming pass, deliberately not applied:**
`MT_TABLE` / `_020AFF80` / `_020AFF88` read as the Mersenne Twister index, the
`mag01` matrix-A table and the 624-word state (**facts** behind it: `MT_TABLE`'s
initialiser is `0x271` = 624 + 1; `_020AFF80` is `{0, 0x9908B0DF}` indexed by
`y & 1`; `_020AFF88`'s extent is exactly 2496 bytes = 624 words, counted off the
`.byte` lines, and the largest displacement used is `#0x9bc` = element 623).

## Other shared-surface changes, none of them layout changes

**Fact**, and worth listing because they widen headers other files include:

| header | added | why |
|---|---|---|
| `include/number_util.h` | `#include "save.h"` | the new prototypes name `struct bitstream`; eight files pick it up transitively |
| `include/moves_3.h` | `#include "common.h"` | `struct ground_move` is defined only there and was not reachable |
| `include/main_02056294.h` | `#include "common.h"` | **required, not cosmetic**: `SetActiveTeam` takes `enum team_id` as a parameter, and an enum tag first seen in a parameter list gets prototype scope and then conflicts with the real one — a hard error |
| `include/main_02055410.h` | `struct ground_monster;` forward declaration | `GetTeamMember` returns it; the header must compile standalone |

No struct, enum, union or typedef other than the two discussed above was added,
widened, renamed or re-typed. In particular `struct team_member_table`,
`struct team_member`, `struct ground_monster`, `struct move_data`,
`struct monster`, `struct entity`, `struct rgba` and `enum team_id` are used
exactly as the tree already defines them, with the target's own arithmetic as
independent confirmation (`0x154 == 5 * 0x44`; `mov r2, #0x1a0` with
`0x1a0 == 4 * 0x68`; `mov r1, #0x1a` for the `move_data` stride; `mov r2, #6`
for the `ground_move` stride).

---

## Open questions for a reviewer

- **The `dungeon.h` grouping is the load-bearing decision in this commit.** The
  codegen evidence for "one object" is strong and comes from two independent
  functions; the *name* and the *meaning* are placeholders. If a later
  pmdsky-debug sync names these fields, the sub-object may need re-splitting, and
  the ledger's falsified list is the record of what a re-split would have to
  solve.
- **`struct unk_022C4C6C::field_0x0` landed as `enum type_id` where the harness
  measured `u8`.** Byte-neutral by the `-enum min` width argument and confirmed
  by the three builds; **whether a scratch was re-measured under the enum
  spelling is not recorded and I did not check.**
- **`struct unk_022C4C6C`'s `field_0x4` is inference** — nothing in this group
  reads it. And the struct's `+2`/`+4` fields overlap two separate `.global`
  symbols (`ov10_022C4C6E`, `ov10_022C4C70`) that other, still-asm code loads
  independently. Two descriptions of the same bytes now coexist.
- **`DUNGEON_PTR` has three spellings in the tree and each is a deliberate
  codegen lever.** Anyone normalising them will silently un-match three
  functions. Whether `[2]` is the right long-term count is itself open: 8 bytes
  is the measured extent, but if a sync labels the second word as its own symbol
  the honest declaration becomes `[1]` or the scalar. All three score 0, so that
  would be a rename, not a re-match.
- **Two counts in the commit message do not match my reading of the diff**:
  "Eighteen call-site declarations" (I count nineteen) and "Three were required
  rather than cosmetic" (I count five that a compiler would reject). Cheap to
  re-check; no effect on the code.
- **`TryWeatherFormChange`'s `const` local is a stand-in, not recovered
  source** — four spellings score 0. The check that would discriminate (do the
  sibling `DungeonGetSpriteIndex` call sites still in asm show the same shape?)
  **was not done.**
- **`u16 *buf` in `sub_02051098` / `sub_020510C0` is inference.** The facts are
  a `add r1, r4, #2` second call and two call sites passing `r7 + 0x44` /
  `r7 + 0x48`. `s16 *` or a two-`u16` struct would reproduce the same bytes;
  both callers are still assembly, so nothing narrows it.
- **`AddExpSpecial`'s first parameter is never used** — `r0` is dead from the
  first instruction. The name and arity come from the pre-existing call-site
  declaration, which all four call sites use. Worth a second look.
- **`AddExpSpecial` never writes `monster::exp`**; it accumulates the clamped
  delta into `unk_exp_tracker` and sets `dungeon::should_enemy_evolve`. That
  reads oddly, is exactly what the asm does, and is consistent with the existing
  comment on `unk_exp_tracker`.
- **Parameter names throughout are descriptive guesses** (`check_blinded`,
  `log_message`, `value`, `base_exp`, `roster_idx`, `a`, `team`). They assert
  nothing the compiler checks; change them freely.
- **Four of the nine groups were measured on the NORTH_AMERICA context only.**
  The target text is asserted byte-identical across regions in each case; the
  step to "EU/JP objects are also right" was closed by the ROM builds, not by
  measurement.
- **`src/overlay_29_022F0EDC.c`'s two K&R externs were left in place on
  purpose** and are weaker than the real signatures.
