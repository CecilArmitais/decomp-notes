# `3fc6d8bd` — Decomp CalcTypeBasedDamageEffects; pad damage_calc_diag to its real layout

| | |
|---|---|
| **Commit** | `3fc6d8bd` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `7b76a1b2` |
| **Verified** | all three ROMs: `pmdsky.us.nds: OK`, `pmdsky.eu.nds: OK`, `pmdsky.jp.nds: OK` — each also passing `main.sha1` and `filesystem.sha1`, so the ARM9 binaries match and not only the packaged ROMs |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The PR diff and the matching build are the sources
> of truth — see [the README](../README.md). Claims are labelled **fact** (read
> off the asm/data, or from an existing in-tree header) or **inference**.

---

Continues the damage cluster from [`5271b77a`](5271b77a.md) /
[`ef68e88d`](ef68e88d.md) (`ApplyDamage`) and [`7b76a1b2`](7b76a1b2.md)
(`ApplyDamageAndEffects`). This one is a **callee**, not a caller: its only
caller is `CalcDamage` (`0x0230BBAC`, 1294 instructions, two call sites), still
asm. Doing it first pins the signature and struct types `CalcDamage` will need
rather than guessing them from a call site.

## Naming policy applied here

| symbol | kept as | why |
|---|---|---|
| `CalcTypeBasedDamageEffects` | unchanged | already named in the tree's asm |
| `ov29_02352838` | unchanged | still an unidentified data symbol; identification was not the assignment |
| the `damage_data` parameter's type | `struct unk_02308FE0` | **reused verbatim** from `overlay_29_02308FBC.h`, where `ApplyDamage` established it |
| the three new padding members | `field_0x5/0x6/0x7` | an offset is a fact; a meaning is not |
| parameter names | descriptive | **inference** — see open questions |

Constants were converted to the tree's existing enumerators, which is not naming
— it is using vocabulary the tree already has, and `-W error` requires it for
enum-typed parameters.

## What the function does

**Fact** (read off the asm): it seeds an out-parameter `fixed_point_64` to 1.0
and multiplies it by every type-, ability-, weather- and status-based modifier in
turn — 24 `MultiplyFixedPoint64` calls. It loops twice over the defender's two
types accumulating a matchup, combines the pair through
`TYPE_MATCHUP_COMBINATOR_TABLE`, then runs a long sequence of independent
guarded blocks. It returns whether the combined matchup is super-effective.

**Inference**: that reading of the flow is mine. The bytes name none of it.

### Corroboration, independent of the diff

Every constant resolved to an existing tree enumerator, and the resulting
semantics are self-consistent. This is evidence the *structure* is right, not
just the bytes — the diff alone cannot distinguish a correct reading from a
coincidentally-equivalent one:

- weather 1 boosts Fire ×1.5 and drops Water ×0.5; weather 4 does the reverse →
  `WEATHER_SUNNY` / `WEATHER_RAIN`. Weather 3 drops every non-Normal type ×0.75
  through `CLOUDY_DAMAGE_MULTIPLIER` → `WEATHER_CLOUDY`. Weather 6 drops
  Electric → `WEATHER_FOG`.
- the type/ability pairs land exactly on the real abilities:
  WATER + `ABILITY_TORRENT`, GRASS + `ABILITY_OVERGROW`, BUG + `ABILITY_SWARM`,
  FIRE + `ABILITY_BLAZE`, GROUND + `ABILITY_MOLD_BREAKER`/Levitate,
  FIRE + `ABILITY_DRY_SKIN`.
- each writes the matching `damage_calc_diag` flag — and Blaze **and** Dry Skin
  both write `fire_move_ability_boost_activated`, which is exactly what that
  field's own comment says.
- `EXCLUSIVE_EFF_HALVED_DAMAGE` (90) pairs with `DAMAGE_MULTIPLIER_0_5`.
- the rodata decodes as `fixed_point_64` 16.16: 0.5, 1.5, 2.0, 0.75, 999.0, 1.0.

## The match

Scratch `current_score: 0`; 612/692 rows byte-identical (the remaining 80 are
the scratch's unlinked-callee rendering, `bl X` vs `bl X-0x8+0x8`, which
decomp.me scores as zero). Progression by *real* differing rows:

57 → 48 → 37 → 34 → 28 → 20 → 9 → 6 → 2 → 1 → **0**.

**The raw score was useless for most of that.** 208 of ~243 differing rows were
length and link shadow — unlinked callees, literal-pool displacements, branch
displacements. Judging by score would have been judging noise, and did mislead
once: a variant scoring 1487 against 1767 had *more* real rows but had actually
fixed the defect being worked. See `docs/MATCHING_TIPS.md` → *Count differing
rows two ways*.

### What each step was

| step | rows | |
|---|---|---|
| `result = FALSE` in the Flash Fire and Levitate arms | 34 → 28 | the target writes `mov r4, #0`, and r4 holds `result`, live to the return — so those arms genuinely assign it. Also semantically right: an absorbed move is not super-effective |
| scalar `DUNGEON_PTR` **and** the two matchup stores made adjacent | 28 → 20 | each alone did nothing; only the pair. The target reloads the pointer between the stores where a cached load would do |
| `static const` templates for the `matchups` init and the `*out` zero | 20 → 9 | a bare `{0, 0}` folds to `mov #0`; the target copies from rodata |
| `*out = <8 bytes>` as a whole-struct copy | 9 → 6 | reproduces the target's load, load, store, store shape |
| the templates reached through the **imported** base symbol | 6 → 1 | see below |
| `s16 wonder_guard` | 1 → **0** | see below |

## `ov29_02352838` — a wrong turn, and what corrected it

**Fact.** Walking `asm/overlay_29_rodata_023527C0.s` with a running address, the
region tiles exactly, 8 bytes per entry, no gaps:

```
+0x00  ov29_02352838 (4 bytes)      +0x24  CLOUDY_DAMAGE_MULTIPLIER
+0x04  DAMAGE_MULTIPLIER_0_5        +0x2c  SOLID_ROCK_MULTIPLIER
+0x0c  DAMAGE_MULTIPLIER_1_5        +0x34  DAMAGE_FORMULA_MAX_BASE
+0x14  DAMAGE_MULTIPLIER_2          +0x3c  <unnamed 8 bytes>
+0x1c  <unnamed 8 bytes>            +0x44  DAMAGE_FORMULA_MIN_BASE
```

The two unnamed entries are what this function reads for its `matchups[2]`
initialiser and its `*out = 0`. They carry no `.global`, so the `.s` dump merged
them into the preceding symbol's byte run — which is why `DAMAGE_MULTIPLIER_2`
and `DAMAGE_FORMULA_MAX_BASE` appear as 16-byte objects when they are 8-byte
fixed-point values.

**The wrong turn.** I first read that tiling as proof the constants were *this
translation unit's* rodata and defined them in the `.c`. It reproduced the
offsets and reached 6 rows, so it looked like progress — but it duplicated data
that already exists and would have required a rodata migration.

**What corrected it** was a reviewer question, and the evidence had been on
screen the whole time: the pool word. Data defined locally emits a
section-relative `.word .rodata`; data imported emits `.word <symbol>`, and the
target has `.word ov29_02352838`. I had dismissed that row as a scratch
artifact. It was the discriminator.

**Fact.** The symbols already exist and are already exported —
`.global ov29_02352838` in the rodata `.s`, `.public ov29_02352838` in
`asm/include/overlay_29_0230AD04.inc`. Nothing needed defining, migrating or
exporting; the C only had to reach it, via the array-base idiom already in the
tips file:

```c
extern const s32 ov29_02352838[];
matchups[0] = ov29_02352838[7];                              /* +0x1c */
matchups[1] = ov29_02352838[8];                              /* +0x20 */
*out = *(const struct fixed_point_64 *)&ov29_02352838[15];   /* +0x3c */
```

**Fact, from the opposite direction**: everything *preceding* `ov29_02352838` in
that rodata file (`ov29_02352810`, `ov29_0235280C`, `ov29_023527F8`) is
referenced by functions in other objects, so the boundary is real.

The multipliers this function names directly (`MATCHUP_*`,
`TINTED_LENS_MULTIPLIER`, `BURN_DAMAGE_MULTIPLIER`,
`TECHNICIAN_MOVE_POWER_THRESHOLD`, `TYPE_MATCHUP_COMBINATOR_TABLE`) live in
**overlay_10** rodata — a different overlay — and are plain externs.

## `struct damage_calc_diag` was three bytes short

**Fact.** `asm/overlay_29_022E0378.s:215` writes `str r5, [r4, #0x188]` and
`asm/overlay_29_022E335C.s` reads `[r6, #0x184]` / `[r6, #0x188]` — **word**
accesses, so `move_type` and `move_category` are four bytes each in retail. And
this function writes `[dungeon, #0x18c]`/`#0x18d`, which is
`last_damage_calc + 0x8/0x9`.

**Fact.** The header modelled `move_type` as a 1-byte enum plus explicit
`field_0x1/0x2/0x3`, but gave `move_category` no padding. Under `-enum min` that
puts `move_indiv_type_matchups` at 0x5 rather than 0x8, and every member through
`attacker_level` (0x16) compiled below the offset its own comment states.
Alignment before `damage_calc` (`s32`, 0x20) re-absorbed the drift, so the
struct still totalled 0x54 and nothing caught it.

**Fact.** The whole tree has only two `last_damage_calc` uses
(`dungeon_damage.c`, `overlay_29_0230AB58.c`), both past the re-convergence
point — so the bug was latent, and the fix is byte-neutral. The matching build
of all three regions is what confirms that, not the reasoning.

## `s16 wonder_guard` — the last row, and an inference

The residue was a one-instruction scheduling swap: the target issues
`ldr r6, [sl, #0xb4]` (`info = GetEntInfo(attacker)`) before `mov r1, #1`
(`IntToFixedPoint64`'s second argument), and the candidate issued them the other
way round. Same instructions, same count.

**Roughly 55 variants did not move it**: every statement order among `info`, the
matchup stores and the call; declaration-with-initialiser for `info`; `info`
first, last and mid in the declaration list; `attacker->info` instead of
`GetEntInfo`; `weather` swept through six declaration positions; `wonder_guard`
as a declaration initialiser and moved one and two statements later; four
chained/reordered spellings of the `wonder_guard`/`field_0xe`/`field_0xf` block;
`ghost_ineffective` and `scrappy` as `s32`; `is_projectile` as `u8`; a `const`
prototype on `MultiplyFixedPoint64` removing 24 casts; the `1` sourced from a
variable and from `TRUE`; and `ov29_02352838` as `u32`, a 1-element array and
non-zero.

The lever was `docs/MATCHING_TIPS.md` → *A stack local's declared WIDTH steers
store scheduling*: a narrow-typed stack local pins its store group to source
order, and this site sits immediately before the prologue's store group.
Sweeping `s32 ↔ s16` one local at a time gave `wonder_guard` → **0**, `power` →
5 (also flips the site), `result` → 1, `i`/`matchup`/`max_hp` → 8/19/24.

**Inference, and the one I would most like challenged.** `wonder_guard` compiles
to a word `str` either way — a small local whose address is never taken is
stored word-wide regardless — so **the bytes cannot distinguish `s16` from
`s32`** at the store. Only the scheduling separates them. The tips entry argues
such retypes read as recovered types (`s16` flag locals are ordinary DS-era
source), and nothing here contradicts that, but it remains a type chosen because
it matches.

## Regions

**Fact**, from surveying every directive in the original asm block: 13
conditionals, all `#ifdef`/`#ifndef JAPAN`, and **no `EUROPE` directive
anywhere** — EU is identical to US.

- six `DefenderAbilityIsActive__0230A940` calls across five `if` sites take
  three arguments under JAPAN and four elsewhere, following
  `src/dungeon_damage.c`;
- five message ids as file-scope `#define`s at US − 0x2C1 under JAPAN,
  following `src/overlay_29_02308FBC.c`;
- the two `DUNGEON_PTR->weather.*_sport_turns` reads shift 0xCD5B/0xCD5C →
  0xCCB7/0xCCB8. **No directive was written for these** — `struct dungeon`
  absorbs the shift through `struct monster`'s own `#ifndef JAPAN` members.
  That started as an expectation and was then measured: the scratch shows
  `ldrb r0, [r0, #0xd5b]` under US and `#0xcb7` under JP, both sides agreeing.

All three regions were scored to 0 on decomp.me **before** any build, with US
run through the same pipeline as a control. They then built matching first time.

## Harness faults hit along the way

Recorded because each produced a mismatch that looked exactly like bad C:

- **A stale context.** `build-tools/mkctx.sh` builds the scratch context inside
  the build image by `git clone`-ing the repo, so it only ever saw *committed*
  headers. The `damage_calc_diag` fix was uncommitted, so several rounds of
  measurement ran against the old struct and showed a plausible wrong offset in
  the candidate. Its copy-in mechanism failed twice over: the header was passed
  as a bare `#include` name rather than a path, and more fundamentally it is a
  *transitive* include that never appears in the argument list at all. It now
  overlays the entire working-tree `include/` unconditionally.
- **A region split on the wrong occurrence.** The region harness cut the
  preprocessed file at the function's *declaration* rather than its definition —
  both start with the same prefix — dragging 2000 lines of enums into the source.
  `rindex`, not `index`.
- **Three zeros needed validating.** Before trusting EU/JP at 0, the
  region-specific rows were checked to be present and genuinely different per
  region. Three identical zeros from a degenerate comparison would look exactly
  like three real ones.

## Open questions for a reviewer

- **`s16 wonder_guard`** — see above. Load-bearing, but the bytes do not
  distinguish it from `s32`.
- **`*(const struct fixed_point_64 *)&ov29_02352838[15]`** is the least natural
  line in the function. It is plain C and it matches, but the spelling is mine.
  If `ov29_02352838` is ever identified as a typed aggregate, this should be
  rewritten as a member access.
- **`DUNGEON_PTR` is declared scalar in this file**, while the adjacent
  `src/dungeon_damage.c` declares it as an array. Both spellings exist in the
  tree (44 files array, 22 scalar). Two declarations in two translation units
  never meet, so **no build can catch the divergence** — only a grep. Scalar is
  what matches here; the array form costs a row. This is the same open question
  the PR note already carries.
- **Parameter names are inferences.** `out`, `attacker`, `defender` and
  `damage_data` are well evidenced; `power`, `attack_type` and `is_projectile`
  are read from how the values are used, not from anything the bytes state.
- **`ov29_02352838` itself is still unidentified**, and this commit does not
  change that. What is now known is its extent and that two unnamed 8-byte
  regions sit inside the run at +0x1c and +0x3c.
- **The three padding bytes** are named `field_0x5/0x6/0x7` to mirror the
  existing `field_0x1/0x2/0x3`. If the maintainers would rather model both enums
  as four-byte members and drop all six placeholders, that is byte-identical.
