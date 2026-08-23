# `ff65ac0e` — Decomp ApplyDamage; fix a message-id parameter type and three field types

| | |
|---|---|
| **Commit** | `ff65ac0e` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `ef8633f9` |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK`; `main.sha1` also passes, so the ARM9 binary matches and not only the packaged ROM |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The PR diff and the matching build are the sources
> of truth — see [the README](../README.md). Claims are labelled **fact** (read
> off the asm/data, or from an existing in-tree header) or **inference**.

---

## Naming policy applied here

| symbol | kept as | why |
|---|---|---|
| `ApplyDamage` | unchanged | already named in the tree's asm |
| the damage-data argument | `struct unk_02308FE0` | new type, named for the function that takes it; naming was not the assignment |
| the two table element types | `struct unk_023528A4`, `struct unk_022C593C` | new types, named for the globals they type |
| every member of all three | `field_0x<off>` | an offset is a fact; a meaning is not |
| `ov29_*`, `ov10_*` callees and data | unchanged | still asm |

pmdsky-debug has names for the first of these (and for several members). They
were **not** used: reading it for layout is fine, importing its vocabulary is a
sync's job. The same reasoning applies to `enum number_color`, which the tree
does not have — `DisplayAnimatedNumbers`' last parameter is declared `s32` and
passed `-1` rather than inventing the enum here.

One type was avoided entirely: `FillRecruitInfo`/`TryRecruit` take a 0x48-byte
block, and `u8 recruit_info[0x48]` passes the same address. **Fact**: verified
byte-identical, so no struct needed inventing.

---

## What the function does

**Fact** (read off the asm): given an attacker, a defender, a damage block and a
damage source, it screens the hit through immunity and absorption checks, logs
the type-matchup and damage messages, plays the hurt animation, subtracts HP with
several floor-at-1 exceptions, and — when HP reaches zero — walks the revival
paths (held item 0x153, held item 0x159 with a neighbouring ally, Reviver Seed,
the bag revival item) before dropping held items, awarding experience, and
either recruiting or fainting the defender.

**Inference**: that reading of the flow is mine; nothing in the bytes names any
of it. The function keeps the name the tree already had.

## The match

Progression: 162405 (stub) → 20902 (full draft) → 3385 → 970 → 375 → 120 → 60 →
**0**. Local scratch `VouSb`.

Four levers did the work after the draft. Each is written up with a worked
example in `docs/MATCHING_TIPS.md`; what matters for review is that none of them
is a trick — each changes the C into a different ordinary shape.

1. **Declaration order.** Moving one declaration (`struct monster *dmon`)
   collapsed a whole-function register rotation: 456 register substitutions →
   14, 48% of rows matching → 76%, 3360 → 970. Four further single moves reached
   375, three of them by *adjacency* — where two registers are transposed,
   declaring the two variables next to each other picks the target's assignment
   (`sl↔r9` → `other`/`i`, `sl↔fp` → `bag_item`/`j`, `sl↔r6` → `amon`/`exp`).
2. **`else` arm first.** The `message_id` select produced the right instructions
   in the wrong order; inverting the test so the `= 0` arm is written first put
   the predicated `movge` where retail has it. 185 → 120.
3. **Pointer locals for three reloads.** Retail reloads a struct byte in three
   places where the candidate cached it. Reading through a `struct item *` local
   reproduces two of them at zero cost (see the open question below for the
   third).
4. **Enumerating the spellings at one site.** With the instruction stream exact
   and only registers differing, the reads at the 0x153 site were enumerated
   over `dmon->held_item.` vs `held_item->` — sixteen combinations, exactly one
   of which puts the loaded byte in retail's register. The same enumeration at
   the 0x159 site (eight combinations) took it to two rows, and dropping a
   temporary in favour of two read-modify-writes closed those.

**Fact**: the twelve final register rows were classified by walking the aligned
diff with a target-register→our-register map, which resolved them to **three**
fresh decisions plus cascades. **Fact**: a single-site edit then removed a row at
a *different* site, because the 0x153 and 0x159 sites shared a live register
across ~0x180 bytes with no intervening call. That is why they were closable one
at a time.

## The tree changes, and the evidence for each

### `struct monster::bide_damage_tally`: `u32` → `s32`

**Fact.** The clamp is `strgt` (signed). `u32` produces `strhi`.

### `struct monster::field_0x168` / `field_0x169`: two `u8` → one `s16`

**Fact.** The boss-experience index is read `ldrsh` at `+0x168`, so the field is
a signed halfword and the two bytes were a mis-split. **Inference**: that it is
an index into `ov10_022C593C` is read from the multiply-by-12 and the table's
stride — the field itself is left as `field_0x168`.

### `struct dungeon_generation_info::music_table_idx`: `u16` → `s16`

**Fact.** Read `ldrsh` at `dungeon+0x40d6`.

### `TalkToSecretBazaarNpcStandard` and siblings: first parameter is a message id

**Fact.** Every caller loads a small constant into `r0` — `0xC6B` from
ApplyDamage, and `0xF32`/`0xF4A`/`0xF4B`/`0xF4C` in
`asm/overlay_29_02344178.s`. **Fact.** The three tree functions are one-line
forwarders, so their parameter types were never constrained by their own bodies;
ApplyDamage is the first C caller. Changed to `s32` across the three forwarders
and the `TalkToSecretBazaarNpc` extern.

**This is a change outside the function being landed**, and the reason it is in
this commit is that `-W error` rejects the call otherwise. It is type-only and
register-identical, and the matching build is what confirms it did not disturb
`overlay_29_022F0590.c`.

### `DUNGEON_PTR` declared scalar

**Fact.** With the tree's usual `extern struct dungeon *DUNGEON_PTR[];` and
`DUNGEON_PTR[0]->…`, MWCC CSEs the pointer load and the function loses three
instructions the target has. The scalar declaration does not.

**This deliberately disagrees with the other files that declare the symbol.**
`src/dg_camera.c`, `src/dg_uty.c` and `src/dungeon_ai.c` all use the array form.
Two declarations of one symbol in two translation units never meet, so **no
build can catch this** — it is flagged here because only a grep would find it.
`extern struct dungeon *DUNGEON_PTR;` is arguably the more honest declaration for
what is a single pointer, and `src/dungeon_ai_items.c` already declares
`BAG_ITEMS_PTR_MIRROR` scalar, so there is in-tree precedent for the spelling —
but a reviewer may reasonably want the tree unified one way or the other.

## The one construct that is not recovered source

```c
EndCurseClassStatus(defender, defender,
                    *(volatile u8 *)&dmon->curse_class_status.curse, 0);
```

**Fact.** Retail compares `[r4,#0xd8]`, branches, and then loads the same byte
again to pass it — with no call, store or redefinition in between:

```asm
bl   ov29_022EAF20
ldrb r1, [r4, #0xd8]
mov  r6, r0
cmp  r1, #2
bne
ldrb r2, [r4, #0xd8]      @ reloaded for the third argument
```

**Fact.** Without the `volatile`, MWCC allocates the value to `r2` from the start
and needs no second instruction — strictly better code, and one instruction short
of the target.

**Inference, and the honest reading**: `volatile` on a game-data field is not
plausible source. It is standard C and no inline asm is involved, so the match is
real, but this line should be read as a stand-in for a construct not yet found.

The closest natural form is a `struct curse_class_status *` local, which reaches
**1623/1624 rows** with `ins=0 del=0 imm=0 reg=0` and differs in exactly one
instruction (`ldrb r2, [r4, #0xd8]` against `mov r2, r1`). Re-deriving the
monster from `defender->info` costs an extra `info` load in either
compare/argument pairing, and five spellings of `EndCurseClassStatus`'s third
parameter (`u8`, `s32`, `u32`, `s16`, `enum status_curse_id`) make no difference
at all. **If a better construct is found, it should replace this line verbatim.**

## Process notes from landing

1. **decomp.me and the real build do not use the same flags.** The preset string
   has no `-W error`; `common.mk` has `-W all -W pedantic -W error`. The first
   build failed on twelve diagnostics the scratch had accepted — eleven implicit
   `int`/`short` → `enum` conversions and one incomplete type. Fixing them
   replaced every raw hex constant with the tree's own enumerator
   (`ITEM_JOY_RIBBON`, `ABILITY_KLUTZ`, `IQ_ITEM_MASTER`, `DIR_CURRENT`,
   `MISSION_TAKE_ITEM_FROM_OUTLAW`, three `EXCLUSIVE_EFF_*`), each checked by
   value against the headers and each re-verified at score 0.
2. **The scratch context harvested its own output.** The context is generated by
   scanning the tree for declarations; once the function was landed it began
   picking up its own `extern int PlayMissSfx__022E611C();` and re-emitting it as
   `(void)`, which broke the next compile. The generator now skips the file the
   function lives in. It failed loudly, but it is the kind of loop that could
   quietly move a context underneath a "verified" result.
3. **A helper of mine destroyed the working source** mid-session by opening the
   file for writing before computing its contents. It was reconstructed and
   re-verified against the recorded score before work continued. Noted because
   the recovery, not the loss, is what the record should show.

## Open questions for a reviewer

- **The `volatile` line above.** The one thing in this commit I would not defend
  as recovered source.
- **`DUNGEON_PTR` scalar vs array.** A deliberate divergence from three other
  files, invisible to any build.
- **`struct unk_02308FE0`'s members.** Offsets `0x0`, `0x4`, `0x8`, `0xC`, `0xE`,
  `0xF`, `0x10` are read by this function; `0xD` and `0x11`–`0x13` are never
  touched here, and nothing in these bytes says whether they are one field or
  four. The struct's size is inferred from the caller's stack usage, not proven.
- **Parameters `a4`, `a5`, `a7`** are unnamed on purpose. `a4 == 1` forces a
  floor of 1 HP and `a5 == 1` gates the experience award; `a7` gates a death
  animation. Those are **inferences** from single call sites in the asm.
- **Signature findings worth carrying to pmdsky-debug**: `ApplyDamage` takes
  **7** parameters, not 6; `ApplyDamageAndEffects` takes 8, not 7; the
  damage-source argument is word-sized (`ldr`, not `ldrh`), so it is not
  `union damage_source`; and `FreeOtherWrappedMonsters` takes a `u32` unique id
  rather than a `struct entity *`. These are suggestions from this function's
  call sites, not upstream-verified.
- **Only the US branch is decompiled.** The original asm block carries **50**
  preprocessor directives, including two wholesale `#ifdef EUROPE`
  duplications. EU and JP were not built and are not expected to fall out of
  this commit unchanged.
