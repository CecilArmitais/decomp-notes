# `decomp-continued` — 278 functions across 60 commits

| | |
|---|---|
| **Branch** | `decomp-continued` |
| **PR** | *none, and none planned — see below* |
| **Base** | `upstream/main` @ `86ec9772` |
| **Commits** | 60 |
| **Functions decompiled** | **278** |
| **Verified** | matching build at every commit, `build/pmdsky.us/pmdsky.us.nds: OK` |
| **Notes written** | **retroactively**, after commit 50 |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The diff and the matching build are the sources of
> truth — see [the README](../README.md).
>
> **These notes were backfilled in one pass, not written as the work happened.**
> They are reconstructed from commit messages and diffs, which are reliable. The
> dead ends, intermediate scores and discarded candidates a contemporaneous note
> would have captured are **lost for most commits**, and nothing here
> reconstructs them by guess. Where a note is silent on what was tried, read that
> as "not recorded", never as "nothing was tried".

## What this branch is, and what it is not

This is **not a pull request branch.** It was opened after the decision to stop
routing work through PRs, which were taking too long to get through review, in
favour of *"going as far as we can"* on one long-running branch. It is therefore
**large, unreviewed, and not shaped for review** — 50 commits touching seven
subsystems, where a PR branch would have been one subsystem.

If any of it is ever proposed upstream, it should be **split by subsystem**
first. The natural seams are the commit clusters in the table below: actor
resolvers, team-member accessors, DSE track events, bag/item accessors, menu
accessors, and the window system.

**Every commit builds matching on its own.** That is the one property that makes
splitting it later feasible.

## Commits

| commit | what | fns | note |
|---|---|---|---|
| `b9893192` | Decomp RandomizeDemoActors; declare DEMO_TEAMS | 1 | [notes](../commits/b9893192.md) |
| `8157b86c` | Decomp sub_02065B14 and GetScriptEntityMonsterId | 2 | [notes](../commits/8157b86c.md) |
| `4463a320` | Decomp four script/palette helpers ahead of the actor resolvers | 4 | [notes](../commits/4463a320.md) |
| `580644e6` | Decomp sub_02065050; make two script-entity fields signed | 1 | [notes](../commits/580644e6.md) |
| `39a8db1b` | Decomp sub_0206549C; make monster_id and a third field signed | 1 | [notes](../commits/39a8db1b.md) |
| `40325982` | Use canonical ground_monster / team_member_table in sub_0206549C | -- | [notes](../commits/40325982.md) |
| `45761ded` | Decomp five team-member accessors | 5 | [notes](../commits/45761ded.md) |
| `ac61cc34` | Decomp five team-member index accessors | 5 | [notes](../commits/ac61cc34.md) |
| `3256b161` | Decomp GetAppointedLeaderMemberIdx and sub_02056914 | 2 | [notes](../commits/3256b161.md) |
| `86d3db8f` | Decomp sub_02055410, sub_02055474 and sub_020554D8 | 3 | [notes](../commits/86d3db8f.md) |
| `6371544d` | Decomp GetUnitNpcIds and GetAdventureNpcIds | 2 | [notes](../commits/6371544d.md) |
| `e1879864` | Decomp sub_020564B0 and sub_0201E380 | 2 | [notes](../commits/e1879864.md) |
| `4f33a43b` | Decomp ov11_022E96E4 | 1 | [notes](../commits/4f33a43b.md) |
| `0f2e0ea8` | Decomp OverlayIsLoaded; migrate its prog_pos_info array to C | 1 | [notes](../commits/0f2e0ea8.md) |
| `e6c5c875` | Decomp five leaf functions; replace four stale prototypes | 5 | [notes](../commits/e6c5c875.md) |
| `26217737` | Decomp seven DSE track-event handlers | 7 | [notes](../commits/26217737.md) |
| `00d4d642` | Decomp the last four DSE track-event handlers in arm9 | 4 | [notes](../commits/00d4d642.md) |
| `3046c8b0` | Decomp six DSE track-event handlers in lib/DSE | 6 | [notes](../commits/3046c8b0.md) |
| `ed153d5f` | Use the established parameter names for the arm9 DSE handlers | -- | [notes](../commits/ed153d5f.md) |
| `909484fb` | Decomp the six DSE LFO track-event handlers | 6 | [notes](../commits/909484fb.md) |
| `acff5cf3` | Decomp the generic-LFO and note-random-region track events | 3 | [notes](../commits/acff5cf3.md) |
| `c356d53c` | Decomp the bank and key-bend track events; restore stripped label addresses | 3 | [notes](../commits/c356d53c.md) |
| `019b4543` | Decomp the volume, expression and pan track events | 5 | [notes](../commits/019b4543.md) |
| `882d2edf` | Decomp the three tuning-delta track events | 3 | [notes](../commits/882d2edf.md) |
| `59fad373` | Decomp three LFO setup track events; complete struct dse_lfo_settings | 3 | [notes](../commits/59fad373.md) |
| `0888986a` | Decomp the tempo and signal track events | 3 | [notes](../commits/0888986a.md) |
| `27c9ec9a` | Decomp four bag accessors; extend struct bag_items past maybeMoney | 4 | [notes](../commits/27c9ec9a.md) |
| `d4b2c2fc` | Decomp two bag helpers that forward to asm callees | 2 | [notes](../commits/d4b2c2fc.md) |
| `399618a4` | Decomp GetItemAtIdx and RemoveEmptyItemsInBag | 2 | [notes](../commits/399618a4.md) |
| `fdbc544f` | Decomp GetNbItemsInBag and IsItemInBag | 2 | [notes](../commits/fdbc544f.md) |
| `3c5de72d` | Decomp IsItemWithFlagsInBag and GetItemIndex | 2 | [notes](../commits/3c5de72d.md) |
| `a49bcedf` | Decomp CountNbItemsOfTypeInBag and HasStorableItems | 2 | [notes](../commits/a49bcedf.md) |
| `06c828e1` | Decomp CountItemTypeInBag and GetEquippedThrowableItem | 2 | [notes](../commits/06c828e1.md) |
| `35134ece` | Decomp twelve game-state and recycle-shop accessors | 12 | [notes](../commits/35134ece.md) |
| `152f6c7e` | Decomp six shop and item helpers; split bag_items' filler | 6 | [notes](../commits/152f6c7e.md) |
| `6f907c68` | Decomp eight veneers and wrappers | 8 | [notes](../commits/6f907c68.md) |
| `3fc19ec7` | Decomp IsEmptyString and four parent-menu accessors | 5 | [notes](../commits/3fc19ec7.md) |
| `fb757337` | Decomp five simple-menu accessors | 5 | [notes](../commits/fb757337.md) |
| `75f2f813` | Restore four literal-pool annotations stripped by earlier commits | -- | [notes](../commits/75f2f813.md) |
| `75c76fa7` | Decomp seven menu paging getters | 7 | [notes](../commits/75c76fa7.md) |
| `363d3edb` | Decomp GetSelectedMenuItemIdx and five window-id wrappers | 6 | [notes](../commits/363d3edb.md) |
| `ec362491` | Decomp six menu and text-box setters | 6 | [notes](../commits/ec362491.md) |
| `f7fbcd80` | Decomp six collection-menu setters; add a second window-contents view | 6 | [notes](../commits/f7fbcd80.md) |
| `80c33c35` | Decomp six storage-selection and notify-note accessors | 6 | [notes](../commits/80c33c35.md) |
| `2472ad54` | Decomp four allocation, keyboard and area-name accessors | 4 | [notes](../commits/2472ad54.md) |
| `aade0cdf` | Decomp GetWindow; retype overlay_31's window callbacks as ids | 1 | [notes](../commits/aade0cdf.md) |
| `643fd53b` | Decomp GetWindowContents, GetWindowRectangle and SetBothScreensWindowsColor | 3 | [notes](../commits/643fd53b.md) |
| `30ea1f34` | Replace the Window placeholder with its real layout | -- | [notes](../commits/30ea1f34.md) |
| `b6faa493` | Decomp NewWindowScreenCheck, SetScreenWindowsColor and the palette getter | 3 | [notes](../commits/b6faa493.md) |
| `7f6977e2` | Decomp ten window accessors, including UpdateWindow and ClearWindow | 10 | [notes](../commits/7f6977e2.md) |
| `702c4c85` | Decomp DeleteWindow and seven window state helpers | 8 | [notes](../commits/702c4c85.md) |
| `59c4a95e` | Decomp the volume and pan fade track events | 2 | [notes](../commits/59c4a95e.md) |
| `4f46fed9` | Decomp thirty adventure-log accessors | 30 | [notes](../commits/4f46fed9.md) |
| `72b366b3` | Decomp SetPokemonBattled and GetNbItemAcquired | 2 | [notes](../commits/72b366b3.md) |
| `26611d66` | Decomp seventeen storage-selection accessors | 17 | [notes](../commits/26611d66.md) |
| `dfbb8236` | Decomp four mission-destination accessors in overlay_29 | 4 | [notes](../commits/dfbb8236.md) |
| `0fbdb572` | Decomp four more mission accessors; find the cause of the batch failure | 4 | [notes](../commits/0fbdb572.md) |
| `5c895f8a` | Decomp nine mission predicates against a faithful scratch context | 9 | [notes](../commits/5c895f8a.md) |
| `bb3a61e8` | Decomp ten dungeon-state accessors in overlay_29 | 10 | [notes](../commits/bb3a61e8.md) |
| `ac3e2389` | Decomp ten more dungeon-state accessors in overlay_29 | 10 | [notes](../commits/ac3e2389.md) |

## Cross-cutting changes a reviewer should weigh

These are the changes that touch types shared with the rest of the tree. They are
the ones most likely to be contentious, and they are collected here so nobody has
to find them across 50 commits.

### Two `-1` sentinels added to shared enums

`enum script_entity_id` gains `ENTITY_NONE = -1` ([`580644e6`](../commits/580644e6.md)),
and `enum monster_id` gains `MONSTER_INVALID = -1` ([`39a8db1b`](../commits/39a8db1b.md)).

**Why:** under `-enum min` an enum with no negative enumerator is unsigned, so
fields of that type compile to `ldrh` where the target has `ldrsh`. An explicit
`(s16)` cast does not help — the 16-bit store folds the conversion away.

**Contentious, and flagged as such.** [`b9893192`](../commits/b9893192.md)
explicitly *declined* to do this, preferring not to diverge from pmdsky-debug.
Two commits later it was done anyway on the strength of nine `ldrsh` reads. Both
sentinels were **verified in isolation** — added alone, nothing else modified,
ROM still matching — so they are byte-neutral for all existing code. But this
is a divergence from pmdsky-debug that upstream may simply not want.

### `struct bag_items` extended three times

- [`27c9ec9a`](../commits/27c9ec9a.md) — four fields appended past `maybeMoney`.
- [`35134ece`](../commits/35134ece.md) — four more, up to `0x13B4`.
- [`152f6c7e`](../commits/152f6c7e.md) — **filler split into three runs** to
  expose two pointers at `0x132C` and `0x1370`.

The first two only append, and cannot move an existing member. **The third
genuinely could have**, and depended on the rebuild of all four translation units
that use the struct to prove `maybeMoney` still lands at `0x1394`.

### Smaller shared-type changes

- `struct dse_lfo_settings` gains `field_0xE` ([`59fad373`](../commits/59fad373.md))
  — converts existing trailing padding into a named field; size unchanged at
  `0x10`.
- `struct Window` replaced with a full `0xE0` layout
  ([`30ea1f34`](../commits/30ea1f34.md)) — **supplied by the maintainer**, not
  derived here. Offsets are checkable; the names are not.
- `struct team_member`'s bitfields were **deliberately not changed**
  ([`6371544d`](../commits/6371544d.md)), at the cost of a raw byte read, because
  no access through the existing bitfield members reproduces the target's single
  `tst`.

### Two views of the same buffer, deliberately not unified

`struct unk_0202AAA8` and `struct unk_0202C5E0`
([`f7fbcd80`](../commits/f7fbcd80.md)) describe the same `GetWindowContents`
buffer with **incompatible layouts** — one reads a byte at `0x1A0` where the
other writes a word, and one writes a byte inside the other's word at `0x1B0`.
They are kept as separate views because a merged struct would have to assert an
agreement the stores disprove.

## Known-imperfect things, listed rather than hidden

### Duplicate declarations the build cannot catch

Two declarations of one function in two translation units never meet, so the
compiler never sees them disagree and the ROM still matches. **Only a grep finds
these.** Known outstanding:

| symbol | where | status |
|---|---|---|
| `DseChannel_SetBank` | new header + `lib/DSE/src/main_02071BF4.c` | agree; collapse when the callee lands ([`c356d53c`](../commits/c356d53c.md)) |
| `MemAlloc` | new header + `overlay_31_02382820.c` | agree; collapse when it lands ([`2472ad54`](../commits/2472ad54.md)) |
| `UpdateWindow`, `sub_02027B1C` | `overlay_25_init.c` declares both as `char *` | **genuinely wrong** — the value is a window id ([`7f6977e2`](../commits/7f6977e2.md)) |
| `UpdateWindow`, `sub_02027B1C` | `overlay_13_0238BDA8.c` declares both as `s8` | harmless; left to preserve an upstream annotation |
| `sub_0202836C` | **five** declarations that disagree: `int`, `s32`, `s8`, `s8`, and `s32` added by [`702c4c85`](../commits/702c4c85.md) | kept out of `window.h` so no overlay sees a conflict |

The `overlay_25_init.c` case is the only *incorrect* one. Fixing it properly
means retyping `ov25_0238B414`'s own parameter and its callers, which is its own
piece of work.

**Closed since:** `DeleteWindow`'s provisional declaration in
`include/main_0202AAA8.h` was replaced by an include when the function landed in
[`702c4c85`](../commits/702c4c85.md), with all three decompiled callers rebuilt.

### Signatures the bytes do not determine

Several functions have signatures that **no scratch can validate**, because the
function's own bytes are identical under every candidate. Where a decompiled
caller exists, it is the evidence; where it does not, the signature is a
convention:

- [`e6c5c875`](../commits/e6c5c875.md) — parameter **count** (settled by a
  matching caller).
- [`b6faa493`](../commits/b6faa493.md) — parameter **width** (got it wrong;
  cost a 33-checksum build).
- [`d4b2c2fc`](../commits/d4b2c2fc.md), [`6f907c68`](../commits/6f907c68.md) —
  forwarder return types and veneer argument lists, **chosen to read sensibly**,
  not read off the target.
- [`7f6977e2`](../commits/7f6977e2.md) — three return/out-parameter types whose
  callers were still asm and were **not read** at the time. **Checked since**, in
  [`702c4c85`](../commits/702c4c85.md): the asm callers pass `ldrsb` ids, compare
  the returns against zero, and hand `sub_020282C8` a stack buffer, so all three
  hold. Not a cascade — but it was luck, not method.
- [`702c4c85`](../commits/702c4c85.md) — two `strb` parameters, same shape, and
  **their callers were not read either**. This is now the most likely place for
  the next cascade.

The pattern is worth naming: **a signature the scratch cannot validate is only
as good as the caller you read.** When no decompiled caller exists, reading the
*asm* caller costs one `grep` and settles it.

### Five functions landed without reaching score 0

In [`7f6977e2`](../commits/7f6977e2.md), five functions score 5 or 10 with
**every instruction matching**; the difference is a literal-pool relocation
*symbol* that resolves to the same address. The in-repo build — which the
project designates the source of truth — matches. The note gives the exact
check that separates this from a real mismatch.

### Work stopped short, with scratches saved

**None of these is a claim that the function cannot be matched.** Retail was
compiled from ordinary C by this toolchain, so a matching source exists by
construction. What follows is **the limit of the attempts made**.

| function | score | scratch | where it stopped |
|---|---|---|---|
| `DseTrackEvent_SetupKeyBendLfo` | **55** | `Z4yLK` | instructions identical, register assignment only. **All 120 declaration orderings and 11 structural variants tried** in [`59c4a95e`](../commits/59c4a95e.md) |
| `DseTrackEvent_TuningFade` | **760** | `3NIIR` | was 935; hoisting `container` before the flag test gained 175. Its `b2 << 8` is emitted `lsl #24` then `lsr #16`, unreproduced |
| `DseTrackEvent_SetLfoParameter` | 505 | `sSRnd` | duplicate-value cases need merging; not attempted since |

**Closed since:** `DseTrackEvent_VolumeFade` and `DseTrackEvent_PanFade` both
landed at score 0 in [`59c4a95e`](../commits/59c4a95e.md), on a declaration-order
swap after seven other spellings all stayed at exactly 620.

**A hypothesis this branch was carrying is now ruled out.** `DeleteWindow`
([`702c4c85`](../commits/702c4c85.md)) was closed by enumerating all 24
declaration orders of its four locals once its instructions already matched, and
this note previously named that as *the untried move* on `SetupKeyBendLfo`. It
has now been tried -- all 120 orderings, plus eleven structural variants -- and
it does **not** close that one. The remaining difference there is driven by
something other than the order the locals are introduced.

Still asm in the window cluster: `NewWindow` (126 instructions), `sub_02027B88`,
`sub_02027E30`, `sub_020278C4`, `sub_02027974`, `sub_0202836C`, and
`sub_02027AA0` / `sub_0202760C` (both of which carry `#ifdef JAPAN` bodies and so
need conditional C).

## Tooling defects found and fixed during this branch

Worth knowing because two of them **silently damaged the tree** in ways no
matching build could report:

1. **`precommit.py` stripped upstream's `; =VALUE` annotations** off lines that
   survived a file split, because its diff parser discarded removals from deleted
   files. 197 lines were affected across the branch; the parser was fixed in the
   workspace repo and the four surviving cases restored in
   [`75f2f813`](../commits/75f2f813.md) — **regenerated from each file's own
   literal pool**, which is exact, after two text-matching approaches were tried
   and discarded for pasting annotations onto wrong lines.
2. **`precommit.py` stripped `; 0xADDRESS` label comments**, which
   `extract_function.py` needs to locate a function. Restored in
   [`c356d53c`](../commits/c356d53c.md); the rule now keeps that form.
3. **`extract_function.py` fails with `Start line None`** on a signature written
   `struct item *Foo(...)` — it parses the name as `*Foo`. Write
   `struct item* Foo(...)`.

## Open questions

- **Do the two enum sentinels belong upstream at all?** They are the branch's
  most invasive change and the one most likely to be rejected on policy rather
  than evidence.
- **`MONSTER_INVALID` vs some other spelling** is a naming call the bytes cannot
  settle; `MONSTER_NONE` was unavailable because `_MONSTER_ID_GENDERED` already
  defines it as `0`.
- **`GetWindowContents` returns `void *`** where the field is typed `u32`
  ([`30ea1f34`](../commits/30ea1f34.md)). Chosen to avoid 22 integer-to-pointer
  conversions at its call sites; a reviewer may prefer the honest `u32`.
- **Many placeholder structs are mapped only where one function touches them** —
  `unk_0201E380`, `unk_02011DF0`, `unk_02032558`, `unk_02028284` among them.
  Their real sizes are unknown and several are probably interior views of larger
  objects.
- **Two species ids in `IsMonsterAffectedByGravelyrockGroundMode` are left raw**
  ([`152f6c7e`](../commits/152f6c7e.md)) and are almost certainly named
  enumerators.
