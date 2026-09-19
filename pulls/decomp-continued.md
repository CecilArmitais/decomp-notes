# `decomp-continued` — 1542 functions across 134 commits

| | |
|---|---|
| **Branch** | `decomp-continued` |
| **PR** | **#290 merged** (through `5ffad87a`), then **#291 merged** (through `6425efdd`, as `440d7b7d`). **Thirty-two commits are unmerged**: the twelve listed previously (`c1a61413` … `240bea14`), plus `2447a020`, `fd3564d3`, `4c7fe636`, `dae72255`, `3fd44e59`, `7e1755dd`, `2cfa1873`, `7f1656cf`, `49da4ee9`, `9b3650fe`, `964beafb`, `92774e8a`, `967fe53b`, `0f3cbcbc`, `6f3ad694`, `637d1966`, `14339589`, `c482e2df`, `d27a7387`, `457a2aaf` and `222afee8`. All sit on top of `a3d64122` and are not yet in a PR. (**Fact**, counted: `git log --oneline a3d64122..222afee8` lists exactly those 33.) |
| **Base** | originally `upstream/main` @ `86ec9772`; after #290 the merge-base was `5ffad87a`; after #291 it was `440d7b7d`. **Rebased 2026-08-24 onto `51c365db`** (upstream PRs #293/#294), then **rebased 2026-09-15 onto `c313f009`** (upstream PRs #295/#296/#297), then **rebased 2026-09-18 onto `a3d64122`** (upstream PRs #298/#299) |
| **Commits** | **134** — the rows of the *Commits* table below, counted. The table is the enumeration; the count is re-derived from it rather than incremented. **It had drifted again**: this cell read 131 while the table held 133 rows, because `d27a7387` and `457a2aaf` were appended to the table without the cell being re-derived. That is the second time this cell has drifted, in the same direction, for the same reason — **re-derive it, never increment it.** |
| **Functions decompiled** | **1542** — the sum of the table's `fns` column, re-derived. This cell had drifted with the one above, reading 1506 while the column summed to 1542; the missing 36 are `d27a7387` (28) and `457a2aaf` (8). `222afee8` decompiles nothing and adds 0. |
| **Verified** | `build/pmdsky.us/pmdsky.us.nds: OK` at every commit. **All three ROMs** at `e0f71917`, `5f96316f`, `38aa265d`, `76533508`, `9097cad3`, `d387d4e2`, `d814f8d0`, `f47593d9`, `240bea14`, `fd3564d3`, `4c7fe636`, `dae72255`, `3fd44e59`, `7e1755dd`, `2cfa1873`, `7f1656cf`, `49da4ee9`, `9b3650fe`, `964beafb`, `92774e8a`, `967fe53b`, `0f3cbcbc`, `6f3ad694`, `637d1966`, `14339589` and `c482e2df`. `33d90d0f` and `2447a020` are US-only on record — for `2447a020` the three-region evidence is decomp.me score 0, not a linked EU/JP ROM, and the note says so. **`9b3650fe` was amended**: as first committed it broke `OVY_13.sbin`, and the gate build that "verified" it had silently built the previous commit |
| **Re-verified after the 2026-09-18 rebase** | all three ROMs at `457a2aaf`: `build/pmdsky.us/pmdsky.us.nds: OK`, `build/pmdsky.eu/pmdsky.eu.nds: OK`, `build/pmdsky.jp/pmdsky.jp.nds: OK`. The per-commit US builds recorded above were run BEFORE the rebase and were **not** re-run per commit afterwards -- only the tip is re-verified. |
| **The tip is now `222afee8`** | `457a2aaf` is no longer the tip. `222afee8` is a three-line identifier rename proved byte-identical at **object** level in all three regions (see [its note](../commits/222afee8.md)), so the three-region ROM result recorded above carries forward to it by construction; a three-region ROM build of that working tree additionally reached `build/pmdsky.jp/pmdsky.jp.nds: OK`. **Stated as an inference, not a measurement**: the US and EU ROMs were not re-checksummed at `222afee8`, because a byte-identical object cannot change the link input. |
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

## What this branch is

It was opened after the decision to stop routing work through PRs, which were
taking too long to get through review, in favour of *"going as far as we can"*
on one long-running branch. It is therefore **large and shaped by that** — 131
commits touching a dozen subsystems, where a PR branch would have been one.
(That figure read 88 when this paragraph was first written; it is the commit
count of the table below, not a separate claim.)

It is now being proposed upstream anyway, at the maintainer's request, to be
reviewed as time allows rather than split up first. **Reviewers should know what
they are getting**: a branch assembled without review pacing it.

If splitting it ever becomes preferable, the natural seams are the commit
clusters in the table below: actor resolvers, team-member accessors, DSE track
events, bag/item accessors, menu accessors, the window system, `SetLeaderAction`
and its neighbours, and twelve file-fragmentation cleanup sweeps.

**Every commit builds matching on its own.** That is the one property that makes
both reviewing it incrementally and splitting it later feasible.

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
| `e8823a88` | Decomp SetLeaderAction in overlay_29 | 1 | [notes](../commits/e8823a88.md) |
| `883be163` | Make SetLeaderAction match EU and JP | -- | [notes](../commits/883be163.md) |
| `f504e6de` | Decomp ten move-effect wrappers in overlay_29 | 10 | [notes](../commits/f504e6de.md) |
| `14f8c511` | Decomp ten more move-effect wrappers in overlay_29 | 10 | [notes](../commits/14f8c511.md) |
| `7c02eea6` | Decomp ten more move-effect wrappers in overlay_29 | 10 | [notes](../commits/7c02eea6.md) |
| `dada4f8d` | Decomp ten more move-effect wrappers in overlay_29 | 10 | [notes](../commits/dada4f8d.md) |
| `edd80366` | Decomp ten more move-effect wrappers in overlay_29 | 10 | [notes](../commits/edd80366.md) |
| `80e48105` | Decomp ten more move-effect wrappers in overlay_29 | 10 | [notes](../commits/80e48105.md) |
| `14d9204c` | Decomp the last ten simple move-effect wrappers | 10 | [notes](../commits/14d9204c.md) |
| `e029b3cb` | Decomp the dungeon RNG mode setters and nine sound helpers | 10 | [notes](../commits/e029b3cb.md) |
| `ba4d07a4` | Decomp ten stat-boost move effects; drop a conflicting extern | 10 | [notes](../commits/ba4d07a4.md) |
| `6174b66b` | Decomp nine more move effects and DebugRecruitingEnabled | 10 | [notes](../commits/6174b66b.md) |
| `c4338759` | Decomp 42 small dungeon-mode functions in overlay_29 | 42 | [notes](../commits/c4338759.md) |
| `1bcd9852` | Decomp eight deferred stat helpers; const StatIndex globals | 10 | [notes](../commits/1bcd9852.md) |
| `185c390e` | Decomp ten message-log accessors in overlay_29 | 10 | [notes](../commits/185c390e.md) |
| `25d5994b` | Decomp ten dungeon utilities; drop a stale extern | 10 | [notes](../commits/25d5994b.md) |
| `b088fd5f` | Decomp five leftover single-function asm files | 5 | [notes](../commits/b088fd5f.md) |
| `80d0e710` | Decomp five more leftover single-function asm files | 5 | [notes](../commits/80d0e710.md) |
| `ec25edd9` | Decomp five more leftover single-function asm files | 5 | [notes](../commits/ec25edd9.md) |
| `0587a6ef` | Correct SetActionUseMovePlayer's parameter types | -- | [notes](../commits/0587a6ef.md) |
| `901ba2aa` | Decomp five more leftover single-function asm files | 5 | [notes](../commits/901ba2aa.md) |
| `78b52f9d` | Decomp five more leftover single-function asm files | 5 | [notes](../commits/78b52f9d.md) |
| `658a92a8` | Decomp four leftover single-function asm files | 4 | [notes](../commits/658a92a8.md) |
| `4d2a484c` | Decomp five leftover single-function asm files | 5 | [notes](../commits/4d2a484c.md) |
| `8cfb04e1` | Decomp five leftover single-function asm files | 5 | [notes](../commits/8cfb04e1.md) |
| `19c62d4b` | Decomp five leftover single-function asm files; fix three stale declarations | 5 | [notes](../commits/19c62d4b.md) |
| `159235da` | Decomp five leftover single-function asm files | 5 | [notes](../commits/159235da.md) |
| `5ffad87a` | Decomp five leftover single-function asm files | 5 | [notes](../commits/5ffad87a.md) |
| `7658e4f9` | Decomp 50 functions in five themed groups | 50 | [notes](../commits/7658e4f9.md) |
| `e865b697` | Decomp 49 functions in five themed groups | 49 | [notes](../commits/e865b697.md) |
| `c34d1196` | Decomp 50 functions in five themed groups | 50 | [notes](../commits/c34d1196.md) |
| `a6ce70e5` | Decomp 50 functions in five themed groups | 50 | [notes](../commits/a6ce70e5.md) |
| `c9d0c766` | Decomp 38 functions across four groups; drop a group to an inlining trap | 38 | [notes](../commits/c9d0c766.md) |
| `1dea0a25` | Decomp 10 DWC functions into lib/src; they need the SDK build rule | 10 | [notes](../commits/1dea0a25.md) |
| `090d9f31` | Decomp 50 functions in five groups | 50 | [notes](../commits/090d9f31.md) |
| `e6530b43` | Decomp 50 functions in five groups | 50 | [notes](../commits/e6530b43.md) |
| `c61f8ec2` | Decomp 49 functions in five groups | 49 | [notes](../commits/c61f8ec2.md) |
| `f968c890` | Decomp 48 functions in five groups | 48 | [notes](../commits/f968c890.md) |
| `be41c2f3` | Decomp 20 functions, emptying six asm fragments | 20 | [notes](../commits/be41c2f3.md) |
| `d9df74f0` | Decomp 17 functions, emptying eight asm files | 17 | [notes](../commits/d9df74f0.md) |
| `6425efdd` | Decomp 16 functions, emptying nine asm files | 16 | [notes](../commits/6425efdd.md) |
| `c1a61413` | Decomp ov11_022ED69C; correct BmaHeader field signedness | 1 | [notes](../commits/c1a61413.md) |
| `eb0db73b` | Decomp ApplyDamage; fix a message-id parameter type and three field types | 1 | [notes](../commits/eb0db73b.md) |
| `e0f71917` | ApplyDamage: match the EU and JP builds | 0 | [notes](../commits/e0f71917.md) |
| `5f96316f` | Decomp ApplyDamageAndEffects; ApplyDamage's damage source is signed 16-bit | 1 | [notes](../commits/5f96316f.md) |
| `38aa265d` | Decomp CalcTypeBasedDamageEffects; pad damage_calc_diag to its real layout | 1 | [notes](../commits/38aa265d.md) |
| `76533508` | Decomp CalcDamage; damage_calc_diag's move_category is 4 bytes, its modifiers unsigned | 1 | [notes](../commits/76533508.md) |
| `9097cad3` | Use generic local names in the five functions landed since 51c365db | -- | [notes](../commits/9097cad3.md) |
| `33d90d0f` | Decomp the fixed-point helper cluster at the end of main_020504BC.s | 5 | [notes](../commits/33d90d0f.md) |
| `d387d4e2` | Decomp ActivateEndOfTurnEffects; monster::bide_move_id is a 2-byte enum move_id | 1 | [notes](../commits/d387d4e2.md) |
| `d814f8d0` | Decomp ApplyItemEffect; replace its stale extern with the new header | 1 | [notes](../commits/d814f8d0.md) |
| `f47593d9` | Decomp sub_0203D538; replace its stale extern with the new header | 1 | [notes](../commits/f47593d9.md) |
| `240bea14` | Decomp ov11_02307334; correct three callee declarations it exposes | 1 | [notes](../commits/240bea14.md) |
| `2447a020` | Decompile GenerateMission (0x0205D224) | 1 | [notes](../commits/2447a020.md) |
| `fd3564d3` | Decompile GenerateExtraHallways (0x0233C9E8) | 1 | [notes](../commits/fd3564d3.md) |
| `4c7fe636` | Decompile CreateGridCellConnections (0x0233E43C) | 1 | [notes](../commits/4c7fe636.md) |
| `dae72255` | Rephrase eight matched functions; drop index casts and magic numbers | 0 | [notes](../commits/dae72255.md) |
| `3fd44e59` | Decompile 16 callees of the combat and dungeon-generation functions | 16 | [notes](../commits/3fd44e59.md) |
| `7e1755dd` | Decompile 11 more callees; give them headers and drop 40 call-site declarations | 11 | [notes](../commits/7e1755dd.md) |
| `2cfa1873` | Decompile 13 more callees; all merge at a file boundary | 13 | [notes](../commits/2cfa1873.md) |
| `7f1656cf` | Decompile 11 more callees; two cheap neighbour clusters | 11 | [notes](../commits/7f1656cf.md) |
| `49da4ee9` | Decompile 12 more callees; clear the head of overlay_11_02308D1C | 12 | [notes](../commits/49da4ee9.md) |
| `9b3650fe` | Decompile 26 more callees; clear six asm files entirely | 26 | [notes](../commits/9b3650fe.md) |
| `964beafb` | Decompile 38 more callees; clear five more asm files entirely | 38 | [notes](../commits/964beafb.md) |
| `92774e8a` | Decompile the head of overlay_11_023090DC; type the parent-menu tables | 5 | [notes](../commits/92774e8a.md) |
| `967fe53b` | Decompile 17 more callees; clear two asm files; enum item_id gains ITEM_INVALID | 17 | [notes](../commits/967fe53b.md) |
| `0f3cbcbc` | Decompile 26 more callees; clear three asm files entirely | 26 | [notes](../commits/0f3cbcbc.md) |
| `6f3ad694` | Decompile 48 more callees; clear four asm files entirely | 48 | [notes](../commits/6f3ad694.md) |
| `637d1966` | Decompile 86 more callees; clear three asm files, split a fourth | 86 | [notes](../commits/637d1966.md) |
| `14339589` | Decompile 119 more callees; clear four asm files entirely | 119 | [notes](../commits/14339589.md) |
| `c482e2df` | Decompile 59 more callees; clear five asm files entirely | 59 | [notes](../commits/c482e2df.md) |
| `d27a7387` | Decompile 28 more callees; clear 21 asm files entirely | 28 | [notes](../commits/d27a7387.md) |
| `457a2aaf` | Decompile 8 more functions; clear four asm files entirely | 8 | [notes](../commits/457a2aaf.md) |
| `222afee8` | Rename three padding members to the documented `field_0x<off>` form | 0 | [notes](../commits/222afee8.md) |

**A note on the last five links.** `967fe53b`'s note was missing at the previous
update of this page and its row said so; it exists now. The four notes for
`6f3ad694`, `637d1966`, `14339589` and `c482e2df` were **being written in
parallel with this update**. Checked, not assumed, at the moment this paragraph
was written: `967fe53b`, `6f3ad694`, `637d1966` and `14339589` are on disk;
**`commits/c482e2df.md` is not yet**, so that one link is written from the
filename convention and will resolve once its note lands. If it 404s, the note
is the thing that is missing, not the link. What a reviewer most needs from
those four is carried in
the cross-cutting sections below, and the evidence behind them lives in the
contemporaneous working notes in the workspace repo, cited by directory name
where each claim is made: `wip/overlay_29_023055B0_all/`,
`wip/overlay_29_02314810_all/`, `wip/overlay_29_022E7EC4_all/`,
`wip/main_0202AB94_all/`, `wip/overlay_29_022E4BB4_all/`,
`wip/overlay_29_0230D088_all/`, `wip/overlay_29_022EA008_all/`,
`wip/main_02055894_all/`, `wip/main_0200F884_all/`, `wip/main_0202F190_all/`,
`wip/overlay_29_022E5650_all/`, `wip/overlay_29_023047DC_all/`,
`wip/main_020504BC_all/`, `wip/main_0200D1F0_all/`, `wip/main_02052060_all/`,
`wip/overlay_29_02320788_all/` and `wip/ov29_022E34A8_EU/`. The earlier
`967fe53b` evidence is under `wip/main_02003328_all/` and
`wip/overlay_29_022FB678_all/`; `0f3cbcbc`'s under
`wip/overlay_29_02337EC0_all/`, `wip/overlay_29_022E335C_all/` and
`wip/overlay_29_0230F02C_all/`.

### A correction to `0f3cbcbc`'s commit message, recorded because the commit was not amended

**Fact, recounted from that commit's own diff.** The message says *"Sixteen
call-site-derived declarations in twelve files are replaced by an include of the
owning header."* It is **nineteen declarations in fourteen files**. The
undercount is in the files holding more than one: `src/overlay_29_02308FBC.c`
and `src/overlay_29_0230BBAC.c` drop three each and
`src/overlay_29_022E4338.c` drops two (a genuine duplicate of the same
`PlayEffectAnimationEntity` block, twice in one file), and
`src/dungeon_recruitment.c`'s `RemoveMonsterFromTile` prototype is written
without the `extern` keyword, so a grep for `extern` does not see it at all.

The full list, per file: `dungeon_logic_2.c` 1, `dungeon_recruitment.c` 1,
`move_orb_effects.c` 1, `overlay_29_022E3F20.c` 1, `overlay_29_022E406C.c` 1,
`overlay_29_022E4338.c` **2**, `overlay_29_022E45B8.c` 1,
`overlay_29_022E4704.c` 1, `overlay_29_022E66C4.c` 1, `overlay_29_022EFA6C.c` 1,
`overlay_29_02308FBC.c` **3**, `overlay_29_0230BBAC.c` **3**,
`overlay_29_0232CA14.c` 1, `overlay_29_02338350.c` 1 — fourteen files, nineteen
declarations. The two `DUNGEON_PTR` lines the same commit rewrites are *data*
declarations retyped in place, not prototypes retired to a header, and are
counted in neither figure.

**Only the message's arithmetic is wrong.** The commit itself is correct, its
three-region build is on record, and nothing about the change depends on the
count. It was **not amended** — amending would re-key `commits/0f3cbcbc.md` and
every reference to that hash — so the discrepancy is recorded here instead, and
a reviewer reading the message should take nineteen/fourteen as the figure.

## Cross-cutting changes a reviewer should weigh

The sections are newest first. The four most recent commits (`6f3ad694`,
`637d1966`, `14339589`, `c482e2df`) land 312 functions and clear sixteen whole
asm files between them, and they carry most of this branch's remaining
shared-type risk — a reviewer with limited time should read from here down to
the `enum item_id` section and stop.

### Rebased onto `a3d64122`, and both sides had decompiled the same function

**Fact.** On 2026-09-18 the thirty-two unmerged commits were rebased from
`c313f009` onto `a3d64122`. Upstream had added six commits: one decompiling 13
functions and emptying six asm files, a review pass, and two pmdsky-debug syncs.
All 32 replayed, `upstream/main` is an ancestor of the tip, and the three ROMs
match at `457a2aaf`.

Four conflicts, each a different kind, and the resolutions are the part a
reviewer should check:

**1. `main_02052A7C` — upstream is a strict superset.** That file held `GetExp`,
`GetEvoParameters` and `GetTreasureBoxChances`. This branch had decompiled only
`GetExp` and split the remainder into `asm/main_02052AB0.s`; upstream decompiled
all three. Upstream's `src/main_0205283C.c` wins outright, and our split
remainder and its `main.lsf` line are gone.

**2. `asm/overlay_29_022E4BB4.s` — our delete plus their rename.** We had cleared
the file; upstream *modified* it, and the modification was a pmdsky-debug rename,
`ov29_022E4C00` → `PlayAttractHitEffect`. The delete is right, but **the rename
had to be carried into our C or the link would break**: upstream's
`asm/overlay_29_02311C28.s:540` now calls the new name and nothing would define
it. Applied to `src/overlay_29_022E4B8C.c` and its header.

**3. `src/main_0205283C.c` — both sides did real work, so neither simply wins.**
Upstream's two new functions use `MONSTER_DATA_TABLE_PTR` as a plain pointer;
commit `c482e2df` had retyped that global into `struct unk_020B09B4` (a grouped
object, load-bearing for `LoadMonsterMd` — see that commit's note). The
resolution keeps **upstream's functions and signatures** and rewrites their 21
accesses onto **our retyped global**. Both survive.

**Consequence a reviewer should know**: `GetExp`'s landed signature is now
upstream's `s32 GetExp(s16 monster_id, s32 level)`, not the
`enum monster_id` this branch used. The one casting call site,
`src/overlay_29_02308FBC.c`, compiles clean against it.

**4. `src/main_02048C5C.c` — an add/add, and upstream's is lighter.** Both sides
decompiled `sub_02048C5C` (ours in `d27a7387`). **Fact**: the target's literal
pool holds `_022AAE74` twice, and MWCC keys pool entries on `(symbol, addend)`
and de-duplicates, so two distinct keys are required. We supplied them by adding
a zero-length `.global _022AAE74_2` to `asm/main_bss_020B3380.s`. Upstream
supplied them with a **negative addend on the next symbol**,
`(u8 *)(&_022AAE78 - 4)` — same two keys, **no change to the assembly at all**.

Upstream's version is what is landed; ours and the alias were dropped, and
`asm/main_bss_020B3380.s` is byte-identical to its original state (verified:
nothing else referenced the alias). **Inference, and the general lesson**: a new
`.global` in a data file is the heaviest way to obtain a second pool key and
should be the last option tried, not the first — a neighbouring symbol with an
addend often does it for free. The `_022AB918` case in `c482e2df` remains a
genuine exception, because there the two objects have *different sizes* and
neither can be expressed as an addend on the other's type.

**What was checked and was not a problem**: a scan for duplicate function
definitions across `src/` lit up ten symbols, but they are forward declarations,
and the pre-rebase tree had 62 of the same. No asm file was cleared by both
sides, and no symbol we removed is still referenced anywhere.

### Rebased onto `c313f009`, and one shared member the two sides disagreed about

**Fact.** On 2026-09-15 the thirty unmerged commits were rebased from
`51c365db` onto `c313f009`. Upstream had added seventeen commits: eight
functions decompiled (`2c2c5c2a`, emptying seven `asm/*.s`), the overlay-30
quicksave work (`src/overlay_30_init.c`, 2218 lines), and a pmdsky-debug sync.

**No function was decompiled twice.** None of upstream's eight is one of ours;
their seven emptied `.s` files are seven this branch never touched.

**Twenty-seven symbols were renamed upstream**, and ten of them are referenced by
files this branch changes — including three functions *this branch decompiled*,
which upstream renamed while they were still asm: `ov29_022E5728` →
`PlayItemThrowSfx`, `ov29_02304A00` → `MakeMonsterIdleInDirectionIfValid`,
`ov29_02320AA4` → `CalcExplosionDamage`. Most of those references live in files
git merges without a conflict, so they would have gone in silently and failed at
link time.

The rename map was **derived twice**, and the first derivation was wrong. Taking
it from the `.inc` diffs gave 18 — it misses `ov29_02320BCC`
(→ `CalcAftermathExplosionDamage`), whose callers are all in its own file, so it
never needed a `.public` line. Re-deriving from every `arm_func_start` / `.global`
definition site gave **27**, and an independent census (symbols present in
`51c365db`'s asm, absent from `c313f009`'s, and not named anywhere in upstream's
`src/`) gave 33 — the extra six are in files this branch never touches.

Every commit was rewritten so it speaks the new names, by **rebuilding each
commit's tree** rather than re-applying its patch: sweeping during a rebase does
not work, because amending commit N invalidates commit N+1's patch context.
Messages, authors and author dates are byte-identical to before. *(Fact:
`git log --format='%an|%ae|%aI|%s'` over the thirty is identical either side.)*

**`struct entity::field_0xaa` looked like a two-sided conflict and was not
one.** It is worth a reviewer's attention because the fix is in a prototype, not
in a type.

- Upstream changed the member `u8` to `s8` in `8c39cbea`. With `s8`, `OVY_29.sbin` failed;
  with `u8`, `OVY_30.sbin` failed. One line, flipping which overlay matched.
  *(Fact: two full three-region builds.)*
- The writer (`src/overlay_30_init.c`) emits `strb` either way; the sign changes only
  *which register* it stores. Both stores sit beside `ov29_022DE9F8(temp_r5)`,
  whose `u8` parameter emits `and r0, r5, #0xff`; under `u8` the store narrows to that
  same zero-mask, so mwcc reuses `r0` and emits `strb r0` where retail has `strb r5`.
- The reader (`src/overlay_29_023047B8.c`) needed `ldrb`, which `s8` does not give --
  **but that was this branch's own bug.** Its call-site-derived prototype for
  `SetAndPlayAnimationForAnimationControl` declared parameters 5, 7 and 8 as `u32`.
  The callee's prologue reads its stacked arguments as `ldrb [sp,#0x18]`,
  `ldr [sp,#0x1c]`, `ldrb [sp,#0x20]`, `ldrb [sp,#0x24]` -- so those three are `u8`.
  With parameter 5 declared `u8`, passing the `s8` member is an `s8`-to-`u8`
  conversion, which mwcc folds into exactly retail's `ldrb`.

**So the member stays `s8`, upstream's `overlay_30_init.c` is untouched, and no cast is
added anywhere** -- only this branch's wrong prototype changes. `s8` is also what the
member *means*: `ov29_022DEA10` tail-calls `ov29_022DE968`, which returns a free-slot
index or `-1` (`mvn r0, #0`).

*(Fact, measured at object level rather than by ROM build: `overlay_29_023047B8.o`
differs between `u8` and `s8` in exactly two loads, and compiling it against the
corrected prototype reproduces the accepted object byte for byte -- sha1
`b4ad8a69`.)* **An earlier revision of this note proposed keeping `u8` and casting the
two stores `(s8)temp_r5`; that also matched, but it was a symptom-level fix that left
the real prototype wrong for every later caller.**

All twelve of upstream's retypes were taken as-is -- `field_0xaa` above, plus
`field_0x188` (`u32`),
`hp_fractional`, `field_0x14`, `force_turn`, `sleep_talk_direction`,
`snore_direction`, `previous_held_item_id`, `field_0xd260`, `monsters` /
`wild_monsters`, `individual_team_spawn_positions`, `highest_enemy_level` — after
checking that nine are read only by `overlay_30_init.c`, that `field_0x14`'s
overlay-29 hits all belong to unrelated structs, and that `field_0x188`'s
signedness is byte-neutral where this branch reads it (the result goes into an
`s32` local before the `>> 8`).

**`monster::bide_move_id` stayed this branch's `enum move_id`** against
upstream's `s16`, and the build settled it rather than an argument: `OVY_30.sbin`
matches with the enum, so upstream's own new code is compatible with it.

**One header change of this branch needed upstream's new file to follow it.**
`struct unk_02337EE8` (see below) moved ten `struct dungeon` members into a
sub-object; `overlay_30_init.c` reaches four of them directly, so those four
expressions gain `field_0x286b0`. Offsets are unchanged, so this is a spelling
change only.

### `struct monster`'s 0x19C–0x20F becomes an array of a new type ([`14339589`](../commits/14339589.md))

**Fact.** 112 lines of `struct monster` — `struct position pos` at `0x19C`, 110
`u8` placeholders, and `u16 walk_anim_frames_left` at `0x1B4` — are replaced by
`struct unk_02304D20 field_0x19c[4]` plus two trailing `s16`. The new type is
`struct position` + five `s32` + `s16` + two `u8` = **0x1C bytes**, and four of
them span `0x19C .. 0x20C`.

**Fact, read off the asm.** Two functions index `monster + 0x19C` with a stride
of `0x1C` (`smulbb`/`smlabb` against `#0x1c`) and a loop bound of 4. Element 0
therefore aliases the old `pos` at `0x19C`, and `walk_anim_frames_left` at
`0x1B4` is element 0 + `0x18` — which is why the old member list could not
produce the indexed access no matter how it was spelled at the read site.

**Fact.** Nothing in `src/` reads either absorbed name through a
`struct monster *` (grep), so the two names disappearing costs no other
translation unit.

**Inference, flagged.** That retail's source had a *four-element array of one
object* here is the reading that reproduces the arithmetic; it is not
established that the original spelled it that way rather than, say, as four
declared sub-objects. The stride, the bound and the aliasing are facts; the
array is the shape that emits them.

**A documentation consequence, flagged rather than fixed.** Both absorbed
members carried comments — *"Mirror of the position on the entity struct"* on
`pos` and *"Number of frames left in walking animation?"* on
`walk_anim_frames_left` — and both are gone with the lines. `pos` survives by
name as the new type's first member; `walk_anim_frames_left` becomes
`field_0x18` and its meaning is now recorded nowhere in the tree. This is the
third case **recorded on this page** of a width or grouping correction silently
deleting the tree's only note on a field (see `speed_boost_counter` under
`0f3cbcbc`, and `bitstream::ptr` below); no census of the branch as a whole has
been run, so there may be more. Claude does not author comments in `pmd-sky`, so
all three are left for someone with full context.

### `struct bag_items` loses `fill2`/`fill3`/`fill4` for the members behind them ([`14339589`](../commits/14339589.md))

**Fact.** Three filler runs totalling `0x1001` bytes are replaced by their real
contents: a `u8`, two 1000-element arrays (`s16[1000]` at `0x38A`, `u16[1000]`
at `0xB5A`), two more `u8`, two `struct bulk_item *` and two 2-D
`struct bulk_item` arrays. `struct bulk_item` moves above `struct bag_items` in
`include/item.h` because it is now a member type. `sizeof` stays `0x13B4` and
every following member — `maybeMoney` included — keeps its offset.

**Fact, each offset read off the asm** (per the commit message): the
`0x300 + 0x8a` and `0xb00 + 0x5a` address splits are forced by ARM addressing
mode 3's 8-bit immediate, `ldrsh` versus `ldrh` fixes the two arrays'
signedness, `cmp #0x3e8` fixes the element count 1000, and `lsl #5` / `lsl #4`
fix the two `bulk_item` strides.

**This is the fourth change to `struct bag_items` on this branch, and the
riskiest.** The three earlier ones (see *`struct bag_items` extended three
times*, below) appended or split filler; this one replaces filler with typed
members across `0x389 .. 0x1394`, and it changes two accessors'
return types — `GetCurrentKecleonShop1ItemByIndex` and
`GetCurrentKecleonShop2ItemByIndex` now return `struct bulk_item *` — which
reaches four files outside the batch.

**Fact, and worth stating plainly because two commit messages disagree.** This
change is in **`14339589`**. `c482e2df`'s message opens a paragraph with
*"struct bag_items loses fill2/fill3/fill4 for the members behind them, so
struct bulk_item moves above it and two accessors return struct bulk_item \*"* —
that paragraph is **stale**, carried forward from the earlier commit. `c482e2df`
touches `include/item.h` only to add `struct unk_0209C850`;
`git log -S'u8 fill2' -- include/` names `14339589` and not `c482e2df`. The
tree is correct either way — the change exists once — but a reviewer reading
`c482e2df`'s message alone would attribute it to the wrong commit. Like the
`0f3cbcbc` count above, this was **not amended**.

**Open, per `wip/main_0200F884_all/STATUS.md` §10.** The outer bound `2` on
`field_0x1330` / `field_0x1374` rests on `cmp r2, #2` in `AllKecleonShopsZInit`
alone, nothing outside the file indexes them, and the placement of
`struct bulk_item` above `struct bag_items` is a presentation call rather than a
measured one.

### `window.h`'s `portrait_params` gains a `Point`, and `ldm` is the evidence ([`14339589`](../commits/14339589.md))

**Fact.** `u32 offset_x; u32 offset_y;` at `0x4`/`0x8` become one
`Point offset;`, and the existing `Point` typedef moves above `portrait_params`
so it can be a member. Same eight bytes, same offsets.

**Fact, measured.** `UpdatePortraitBox` loads the pair with
`add r1, r4, #0xc; ldm r1, {r1,r2}`. MWCC emits `ldm` for this only when the
base register points **exactly** at the object being passed; two separate member
reads scored **860**. This is the same family of lever as
`struct unk_02337EE8` in `0f3cbcbc` and `struct unk_022FBD24` in `964beafb` —
grouping members into a type is what produces the target's base-register
arithmetic — but here the tell is a block load rather than an add chain.

**Fact, measured, and this is why it is safe.** `window.h` is reached by **25
translation units**; every object among them is byte-identical before and after.
That is a measurement, not an argument from "the offsets did not move".

**Inference, flagged.** The two absorbed members' comments
(*"Tile offset (x / 8) …"*) went with them; `Point`'s `x`/`y` carry the meaning
adequately, but the "/ 8" scaling is now undocumented.

### `struct bitstream::ptr` is `u8 *`, not `char *` ([`c482e2df`](../commits/c482e2df.md))

**Fact.** `include/save.h` changes `char* ptr; // Current byte` to `u8* ptr;`.
**Fact, and this is the whole argument:** `CopyBitsTo` and `CopyBitsFrom` read
through it with **`ldrb`**, and the build compiles with `-char signed`, under
which `char *` emits `ldrsb`. No cast at the read site fixes that — MWCC takes
the load's width *and* signedness from the declared type.

**Worth a reviewer's attention out of proportion to its size**, because
`struct bitstream` is a save-system type and the change is a *signedness*
change on a pointer that other code may deref. The three-region build is the
evidence that nothing else depended on the old spelling; no separate grep census
of `bitstream::ptr` users is recorded here, so treat "nothing else depended on
it" as **build-confirmed rather than surveyed**. The `// Current byte` comment
went with the line.

### A second bss label at `0x022AB918`, and no bytes moved ([`c482e2df`](../commits/c482e2df.md))

**Fact.** `asm/main_bss_022AB0AC.s` gains two lines — `.global _022AB918` and
the label `_022AB918:` — immediately above the existing `TEAM_NAME` label, at
the same address. **No `.space` is added**, so no later symbol moves and the bss
layout is unchanged. Confirmed by building that one-line change alone against
the previous commit, per the commit message.

**Fact, and the reason it is needed.** Two objects of different size share that
address: the record `TEAM_NAME` (`0x14` bytes, `0x10` under `JAPAN`) and its
leading name buffer (`0xC`, `8` under `JAPAN`). Four functions load both, which
needs two distinct `(symbol, addend)` literal-pool keys; a single symbol yields
one pool word and leaves the pair four bytes short.

**Open, and the batch's own note says it is the thing most worth pushing back
on** (`wip/main_020504BC_all/STATUS.md`): **which of the two objects is really
`TEAM_NAME`** is *not* determined by anything measured. The two-symbol model is
forced by the pool; the assignment of the existing name to one of them was
chosen because it keeps five other functions at 0. **Not checked:**
pmdsky-debug has no data symbol anywhere in `0x22AB910 .. 0x22AB930`, so there
is no upstream name for the second object and `_022AB918` is the ordinary
`_0<ADDR>` placeholder, claiming nothing.

**An alternative that needs no bss change, recorded rather than recommended:**
declare `extern u8 _022AB924[];` and reach the struct as
`((struct unk_022AB918 *)(_022AB924 - 0xc))`. The `A - 0xc` fold is measured, it
produces the same two pool entries and the same score, and it was rejected as
strictly uglier.

### `DUNGEON_PTR`: two more files take the complete type; the census is now three spellings across 92 files

**Fact, counted by grep after `c482e2df`:** **57** files declare
`extern struct dungeon *DUNGEON_PTR[];`, **31** declare the scalar, and **4**
declare `*DUNGEON_PTR[2]` (`src/dg_camera.c`, `src/special_move_types.c`,
`src/overlay_29_022E9FC0.c`, `src/overlay_29_023047B8.c`). It was 52 / 31 / 2 at
`0f3cbcbc`; the four commits since add five new `[]` declarations and two new
`[2]`. **No header in `include/` declares the symbol at all** — the one
`include/` hit is not a declaration of it — so every spelling is local to its
translation unit and the disagreement stays invisible to the build.

**Fact, measured, and it is the same lever the `0f3cbcbc` section below
describes.** `ov29_022EA008` scores **1025** under the incomplete `[]` form: an
incomplete type lets MWCC keep `DUNGEON_PTR[0]` live across a store the target
reloads after. `[]` and `[2]` are compatible types, so adopting `[2]` changes no
use site in either new file. `src/overlay_29_023047B8.c` took it for the same
reason.

**What this does to the open question.** Four files now needing a complete type,
found independently in three separate commits, makes this look less like a
one-off and more like the default MWCC behaviour that the 57 remaining `[]`
files simply do not exercise. That is **inference** — none of those 57 has been
measured under any other spelling — but it shifts where the burden of proof
sits: the question is no longer "why do these few files differ" so much as "how
many of the 57 would also match under a complete type". Nobody has run that
measurement, and every change of spelling is a per-function re-measurement.

### MWCC inlines a callee it has already seen — the second time, with the opposite remedy ([`c482e2df`](../commits/c482e2df.md))

`967fe53b` split one asm file into two objects because `GetTime`'s calls to a
preceding `EnableAllInterrupts` were folded away (section below). `c482e2df`
hits the same rule and has to go further: `sub_02050CF8` and `sub_02050D84`
land in **translation units of their own**, `src/main_02050CF8.c` and
`src/main_02050D84.c`.

**Fact.** Both call `BitstreamDebug`, which is four bytes — an empty body — and
which is one of the other 26 functions of the same asm file. Compiled beside it
at `-O4,s`, MWCC inlines the call away and each function comes out **two
instructions short**; the ROM then shifts and **33 of 37 sbins stop matching**.
Retail emits a real `bl` there.

**Inference, and a strong one:** retail compiled these two functions apart from
`BitstreamDebug`. The bytes establish that the call was not inlined; they do not
establish the file boundary that produced that, and as with `967fe53b` any
boundary that separates them would do. **Fact, recorded because it was the
obvious wrong suspect:** the literal pool — five words and the predicated
`ldrne`/`ldreq` pair — was never the problem.

**The transferable rule, now seen twice:** when a decompiled function is short
by exactly the instructions of a call, check whether the callee is an
already-compiled function in the same translation unit before touching the C.

### An asm object created in one commit and removed in the next ([`637d1966`](../commits/637d1966.md) → [`c482e2df`](../commits/c482e2df.md))

Worth following as a unit, because reading either commit alone gives the wrong
impression.

**Fact.** `637d1966` clears 22 of `asm/main_02055894.s`' 23 functions and
**splits** the file rather than clearing it: `sub_020559D8` sat at score **75**
with **0 structural rows** — same 82 instructions in the same order — and 11
register-colouring rows, so it was left as its own object `asm/main_020559D8.o`
between `src/main_02055770.o` and `src/main_02056294.o`. That is the branch's
stated standard applied honestly: a near-match is not landed.

**Fact, measured** (`wip/main_02055894_all/STATUS.md`, `LEDGER.md` §D.5–D.8).
Three values sat in a rotated set of callee-saved registers (a block-copy temp
on `r6` where the target has `r5`, `j` on `r7` where the target has `r6`, `dst`
on `r5` where the target has `r7`). **All 24 declaration orders of
`{m, i, dst, j}`, all 120 of `{m, i, dst, j, w}` with the copy temp named, and
284 scope × statement-order variants — roughly 450 measured spellings — never
moved them.** It closed in `c482e2df` on **one line in the trailing loop, the
one furthest from the eleven differing rows**: `dst->is_valid = 0; dst++;`
respelled `dst++->is_valid = 0;`. Both spell the identical instruction,
`strb r1, [r7], #0x44`; only the register assignment changes. The object created
one commit earlier is deleted again.

**The transferable part, and it generalises the lesson already recorded under
`92774e8a`:** a declaration-order sweep is only as general as the *set* it
sweeps, and the discriminator here was not a declaration lever at all. The
respelling that fixed it is in a statement that was not in any differing row.

### Smaller shared-type changes in the four newest commits

The first four are width or layout corrections forced by a load the previous
declaration cannot produce; each replaces adjacent placeholders with one wider
member at the same offset, so no following offset and no alignment moves. The
last three are not width corrections — a header move, a filler split, and a
parameter retype — and are grouped here only because they are small. All are
confirmed by three-region builds, and all are **facts read off the asm** except
where noted.

| change | commit | evidence |
|---|---|---|
| `struct ground_monster`: `level`, `level_at_first_evo`, `level_at_second_evo` `s8` → `u8`; `iq`, `max_hp` `u16` → `s16` | `637d1966` | `sub_02055E14` reads the first three `ldrb` and the last two `ldrsh`. **The reading validates itself**: `id` at `0x4` is read `ldrsh` and is already declared `s16`, and every member whose declaration is already correct reads exactly as declared. A cast at the read site cannot fix this. **Deviates from pmdsky-debug's `s8`/`u16`**, and it narrows `ApplyGummiBoostsGroundMode`'s second parameter from `u16 *` to `s16 *` — that function is still asm, so its prototype is now pinned by this reading rather than by its own body |
| `struct dungeon`: `u8 field_0x1c; u8 field_0x1d;` → one `s16` | `637d1966` | `ldrsh`/`strh` at `0x1c`. `0x1C` is 4-aligned and `0x1E` is already `s16`, so no padding appears. No other TU names either field |
| `struct entity`: `field_0xac` + `field_0xad` → one `s16` | `14339589` | `ov29_02304830` reads `entity + 0xAC` with `ldrsh`, which no pair of `u8` members can produce and no cast at the read site can fix. Neither name is used anywhere else |
| `struct unk_0202AAA8` gains four members carved out of existing byte padding (a `u32` at `0x0`, a flag word + initial selected index at `0xFC`/`0x100`, a `u8 *` at `0x160`, an `s32` at `0x1A8`) | `6f3ad694` | offsets and `sizeof` (`0x1C8`) unchanged, **proven by compiling one `offsetof` assertion per member with `mwccarm` and reading the folded constant back — with a negative control that correctly fails**, so "all as expected" is an outcome the check could have missed. Nothing in the tree read any of the four offsets |
| `struct struct_2` moves from `include/overlay_31_02382820.h` to `include/window.h` | `6f3ad694` | a pure move; the name is pre-existing and unchanged. `CreateSimpleMenuInternal` copies it whole (`0x98` bytes), so its size is load-bearing. `window.h` rather than `main_0202AAA8.h` because the latter drags `main_0202AB40.h`'s prototypes into `overlay_31_02382820.c`, where they collide with that file's own declarations |
| `struct unk_020517F4`: `u8 field_0x0[8]` → `u8 field_0x0[4]` + `s32 field_0x4` | `c482e2df` | a 32-bit load at offset 4 |
| `sub_02051FF0` and `LoadFileFromRom` take `const char *path`, not a `u32` file id | `c482e2df` | `src/ground_bg.c` already declared the latter correctly — i.e. the tree already disagreed with itself about this parameter |

Also in `6f3ad694`, four data symbols that a call site had declared
`extern s32` (because the consuming parameter was `void *`) are given their real
types: `_0209C85C` and `ov11_02322CC8` are `WindowTemplate` — **fact, measured:**
each spans exactly `0x10` to the next symbol and `WindowTemplate` is 16 bytes —
and `_0209C86C` and `ov11_02322CF0` are `struct unk_0202A5CC[]`, 8-byte records
matching the element counts the call sites pass. Each had exactly one user. This
is the same failure mode as the parent-menu tables in `92774e8a` below: **a
`void *` parameter will accept any wrong type, so the build cannot see the
error and only the data settles it.**

### A EUROPE-only function, and a tool that leaves an orphan `#endif` ([`c482e2df`](../commits/c482e2df.md))

**Fact.** `ov29_022E34A8_EU` exists only in the EUROPE build. Its definition and
its prototype both carry `#ifdef EUROPE`, and its single call site was already
inside one. `asm/overlay_29_022E34A8.s` is renamed
`asm/overlay_29_022E2B68.s` for the remaining eight functions.

**Fact, a tooling defect worth knowing before the next region-guarded
extraction.** Because the function was the **first** in its `.s` *and* sat
inside a region conditional, `extract_function.py` took the opening `#ifdef`
with the function and left the closing `#endif` behind in the remainder file.
The landed diff shows both directives removed, so it was cleaned up by hand
here; the tool was not changed. See also the tooling list further down.

### Declaration hygiene across the four commits: 65 call-site declarations retired, at least 21 of them wrong

**Fact, from the commit messages.** `6f3ad694` retires **9** across six files,
`637d1966` **35** across nineteen files, `14339589` **4**, `c482e2df` **17** —
each replaced by an `#include` of the header that now owns the function, or
corrected in place. The "at least 21 wrong" is a floor, not a count: all nine of
`6f3ad694`'s were wrong and eleven of `637d1966`'s, and at least one of
`14339589`'s four; `c482e2df`'s seventeen are not broken down in its message and
have not been re-derived here.

**Fact, the ones that were wrong, not merely redundant:**

- `6f3ad694`: four declared `void` where the function returns `bool8`; two
  declared an `s8` return where it is `s32`; three took an `s8` parameter where
  it is `s32`. **One was the fifth `extern` on a single physical line, which a
  line-oriented grep does not see** — worth knowing, because the declaration
  censuses recorded on this branch have all been greps.
- `637d1966`: **eleven** disagreed with the definition — `UnkMapRelatedFunc`
  declared with an unsigned first parameter in four files where the switch
  dispatch is a signed range test, six declared with no prototype at all and the
  wrong return type, and `sub_02055894` declared as taking a `u32` where it
  takes a pointer.
- `14339589`: `PlayEffectAnimationPixelPos` was declared taking a
  `struct entity *` and returning `void`; it takes a `struct pixel_position *`
  and returns `s32`, so `ov29_022E563C`'s own signature changes with it. Two of
  the four removed declarations were **packed onto one physical line** in
  `src/overlay_11_02307334.c`.

**Both of the grep-evading forms above are new information about the census
method**, not just about these files: a second declaration on the same physical
line as a first, and a prototype written without `extern`, are invisible to the
greps this branch has used — the latter is exactly what made `0f3cbcbc`'s own
count wrong (see the correction after the commits table).

**The recurring point, which this page has now made in several separate
sections:** two declarations in two translation units never meet, so the
compiler cannot see them disagree and the ROM matches either way. Only a grep
finds these, and only a careful grep finds all of them.

### `enum item_id` gains `ITEM_INVALID = -1` — the branch's third negative sentinel ([`967fe53b`](../commits/967fe53b.md))

**Fact.** `include/item.h`'s `enum item_id` now opens `ITEM_INVALID = -1,` above
`ITEM_NOTHING = 0`, placed exactly as `MONSTER_INVALID` is in `enum monster_id`.
The enumerator range becomes −1 … 1399, which still fits two bytes under
`-enum min`, so no field of this type changes width.

**Fact, measured.** `AuraBowIsActive` (landed one commit later, in `0f3cbcbc`)
scored **200 with exactly one differing row** against the tree as it stood:
the target loads `monster->held_item.id` with `ldrsh` where the candidate
emitted `ldrh`. The load's sign is decided by the *parameter* type —
`HasHeldItem(struct entity *, enum item_id)` — and under `-enum min` an enum
with no negative enumerator is unsigned.

**Fact, measured, and this is the part worth a reviewer's time.** Four ways of
fixing it at the call site were tried first and **all four still emitted
`ldrh`**: an `(s32)` cast, an `s32` local, an `s16` local, and an
`enum item_id` local (`wip/overlay_29_0230F02C_all/STATUS.md` §3). Adding the
enumerator to a *copy of the generated context*, with the function body
untouched, took it to score 0.

**Verified in isolation** (per the commit message): the enumerator was added
alone, nothing else changed, and all three ROMs still matched — the same
standard `ENTITY_NONE` and `MONSTER_INVALID` were held to earlier on this
branch.

**The knock-on, which the scratch could not have predicted.** A decomp.me
scratch does not carry the build's `-W …` flags. With `enum item_id` now
*signed*, `-W error` rejects the `short` → `enum item_id` conversion at the
`HasHeldItem` call, so the landed body in `src/overlay_29_0230F810.c` carries an
explicit cast the scratch never needed:

```c
return HasHeldItem(entity, (enum item_id)monster->held_item.id);
```

**That cast is there for the diagnostic, not the codegen** — the function scores
0 with or without it, the codegen was already right once the enumerator existed.
It is easy to confuse with the `(s32)` cast above, which was measured *not* to
fix the load; they are different casts for different purposes.

**Untested alternative, recorded rather than recommended.** `include/item.h`
declares `s16 id; // 0x4` in `struct item`, which is why the call needs a
conversion at all. Typing that member as `enum item_id` would remove both the
conversion and the cast. **Nothing about it has been measured**: `struct item`
is used wherever items are, every existing `*(s16 *)&…` cast around an item id
is a site that could move, and `struct item_volatile` is a parallel declaration
of the same layout (`volatile s16 id; // 0x4`, kept to match `AiDecideUseItem`)
that would have to change with it.

### `struct dungeon` gains a second sub-object: `struct unk_02337EE8` ([`0f3cbcbc`](../commits/0f3cbcbc.md))

**Fact.** Ten consecutive `struct dungeon` members, from `group_id_copy` at
`0x286B0` through `spawn_table_entries_chosen`, are grouped into a new
`struct unk_02337EE8`; the parent gains one member
`struct unk_02337EE8 field_0x286b0;` in their place, same order, same offsets.
Layout-preserving **by measurement**: `sizeof(struct dungeon)` and 17 offsets in
and around the block come back unchanged, flat versus nested, in all three
regions (`wip/overlay_29_02337EC0_all/cand/census_verify.py`).

**Fact, measured.** The type alone is not the lever. Written inline,
`dungeon->field_0x286b0.spawn_entries_master[i]` still folds member-of-member to
a single constant offset and still scores **225**. What reaches 0 is the
sub-object **plus a local of its pointer type**:

```c
struct unk_02337EE8 *p = &dungeon->field_0x286b0;
...
    GetMonsterIdFromSpawnEntry(&p->spawn_entries_master[i]);
```

which emits the target's four adds — `#0x6b0`, `#0x28000`, `#0x2b4`, `#0x4000` —
with **no constant written anywhere in the source**. `ov29_02337EE8` and
`GetRandomSpawnMonsterID` both reach score 0 this way in all three regions.

**Fact.** Eleven other expressions in four files (`src/overlay_29_022EFA6C.c`,
`src/random_trap.c`, `src/spawn_1.c` ×7, `src/spawn_2.c`) gain `field_0x286b0.`
after the `->`. Each compiles byte-identical both ways in all three regions —
but the edit is **not optional**: the tree does not compile without it. No
`asm/` file names a struct member, so `asm/` needed nothing. The comments on the
moved members travelled **verbatim** with them, which is the one mechanical
exception the no-comments rule allows.

**Inference, flagged.** That the original source held a pointer to an object
beginning at `group_id_copy` is *not* established. The bytes rule out the
cast-free spellings and nothing more; six other spellings carrying a cast were
measured to emit the same four adds.

**This is the second time on this branch that regrouping existing `dungeon`
members is what produces the target's base-register arithmetic** — the first was
`struct unk_022FBD24` in [`964beafb`](../commits/964beafb.md), below. A reviewer
weighing one should weigh both; they are the same argument twice.

### Two width corrections, and a pmdsky-debug field that is described correctly at the wrong address ([`0f3cbcbc`](../commits/0f3cbcbc.md))

Each replaces adjacent `u8` members with one wider member at the same offset, so
**no following offset and no struct alignment moves**.

| member | was | now | evidence (all fact, read off the asm) |
|---|---|---|---|
| `dungeon::number_completed_floors` (`0x1E`) | `u8`, plus `u8 speed_boost_counter` at `0x1F` | `s16`, absorbing `0x1F` | read `ldrsh [.., #0x1e]` by `ov29_022E335C`, by `DisplayFloorCard` (`asm/overlay_29_02348020.s:223`) and at `asm/overlay_29_0233544C.s:583,1019` — all three pairing it with `ldrb [dungeon, #0x749]`, i.e. `floor`. Written `strh` at `asm/overlay_29_022E6928.s:545`, whose next three instructions compute `[0x22] = [0x20] + [0x1e]` — what `dungeon.h` already documents `total_floors_completed` as being set to |
| `monster::field_0x188` | four `u8` (`0x188`–`0x18b`) | one `s32` | read `ldr` twice in `PlayEffectAnimationEntity`; `asm/overlay_29_02318AD4.s:30,73` loads it `ldr` and compares against `0xC800`; written `str r5, [r4, #0x188]` at `asm/overlay_29_022E0378.s:215`, where `r4` is unambiguously a monster. `0x188` is 4-aligned |

**Fact.** No other C source reads either member: `grep` for
`number_completed_floors` / `speed_boost_counter` found only the declarations
themselves, and the only `field_0x188` hits are an unrelated struct of the same
member name in `src/main_0203D538.c`. The four `monster` members sit outside
every region `#ifdef`, so one edit serves all three regions.

**Two documentation consequences, flagged rather than fixed.** Deleting
`speed_boost_counter` also deleted its comment — *"Turn counter, Speed Boost
triggers every 250 turns, then the counter is reset"* — and the surviving comment
on `number_completed_floors` still reads *"odd it is not a u16 like the others"*,
written when the member was `u8` and now stale against the `s16` beneath it. Both
were left alone because CLAUDE.md forbids authoring comments in `pmd-sky`;
someone with full context should decide the wording.

**Resolved 2026-09-19 — and this one is worth taking back to pmdsky-debug.** The
question this section used to leave open (*whether absorbing `speed_boost_counter`
is a deviation worth raising upstream*) has been answered: **the Speed Boost
counter is real, and it is not in `struct dungeon`.** It is a byte on
`struct monster` — `+0x11F` in NORTH_AMERICA and EUROPE, `+0x11B` under JAPAN via
that file's own `OV29_0230FC24_OFFSET` region macro — incremented, tested and
reset by `ActivateEndOfTurnEffects` (`asm/overlay_29_0230F9A4.s:766-784`) under
ability `0xB`, against `SPEED_BOOST_TURNS` = `.byte 0xFA` = **250**
(`asm/overlay_10_rodata_022C464C.s`). The byte is already in the tree, unnamed, as
`struct monster::field_0x11f` (`include/dungeon_mode.h:405`). That reproduces
upstream's sentence term for term at a different address, so pmdsky-debug appears
to have attached a real, correctly-described mechanic to the wrong offset. Nothing
true about `dungeon` 0x1F was lost by absorbing it.

Three further facts close the merge, all established after the commit was made
and written up in [`0f3cbcbc`](../commits/0f3cbcbc.md) (2026-09-19 addendum):

* **Alignment forces the direction.** A 16-bit member must be 2-byte aligned, so
  the halfword can only begin at 0x1E (even), never 0x1F (odd). It was never
  possible for `speed_boost_counter` to be the 16-bit field.
* **Nothing accesses `dungeon` 0x1D or 0x1F at any width.** A complete census of
  every immediate-offset byte access at those offsets in the `a3d64122` asm tree
  (80 + 38 sites) finds zero `struct dungeon` accesses. The caveat — ~95 sites
  outside dungeon-mode code were classified structurally rather than hand-traced
  — is recorded in the commit note, along with a warning that the *first* version
  of that census was unsound in the worst place (its regex excluded `sb`/`sl`/`fp`
  bases, which is exactly where a long-lived dungeon pointer lives).
* **The initialisation writes both bytes at once** — `strh r6, [r0, #0x1e]` with
  `r6 = 0` zeroes 0x1E and 0x1F together. An independent counter at 0x1F would be
  silently reset on every dungeon load.

**The sibling merge at 0x1C has stronger evidence still, and it is upstream's
own.** `src/overlay_29_022F0EDC.c:191`, landed and inside the matching ROM at
`a3d64122`, reads `*(u16 *)&DUNGEON_PTR->field_0x1c = 0;` — a cast that exists
only because `u8 field_0x1c; u8 field_0x1d;` cannot express the `strh` retail
performs. Upstream hit the same wall and worked around it at the use site. See
[`637d1966`](../commits/637d1966.md). If these merges are accepted, that cast
should be removed with them.

**Still open, and deliberately:** the *name* `number_completed_floors` remains
upstream's guess. What is proved is width, signedness, and what the field is
arithmetically used for. Mildly against the name: it is 0x1E, not 0x20, that is
added to the floor byte at 0x749 in `ov29_022E335C`, `DisplayUi` and
`DisplayFloorCard` — which sits oddly with the upstream descriptions of both
members. Renaming would need its own investigation; neither merge depends on it.

**A prediction for a future contributor**, recorded here because it will surface
as a mismatch rather than as a compile error: `asm/overlay_29_022E6928.s:547`
reads `dungeon + 0x20` with `ldrsh`, while the tree declares
`u16 number_preceding_floors`. Expect to need `s16` when
`LoadMappaFileAttributes` is decompiled. Nothing contradicts `u16` today only
because that function is still assembly.

### `DUNGEON_PTR`: the lever is complete-vs-incomplete type, not scalar-vs-array ([`0f3cbcbc`](../commits/0f3cbcbc.md))

**Fact, measured** (`wip/overlay_29_022E335C_all/cand/ptr-form.md`, a full
sweep of declaration × access spelling, real compiles, NORTH_AMERICA):

| declaration | score for `ov29_022E34C8` |
|---|---|
| `extern struct dungeon *DUNGEON_PTR[];` | **815** under all three access spellings tried (`DUNGEON_PTR[0]->`, `(*DUNGEON_PTR)->`, `(*(DUNGEON_PTR + 0))->`) |
| scalar, `[1]`, `[2]`, `[3]`, `[4]`, `[8]` | **0** under every access spelling tried |
| either form with `const` | 875 |

**Fact.** The access spelling is not the lever, and once the declaration is
right the body barely matters: 7 of 8 tail shapes score 0, including one with no
pointer local at all. The single exception reads the global once and caches it,
which is a semantically different program.

**Inference, not verified against compiler internals.** What moves MWCC is the
**completeness** of the declared type. An incomplete `extern T *G[];` is what
licenses keeping the loaded pointer live across an intervening store; give it
any complete type and the load is re-issued, after which the field store can no
longer be forwarded to the test either. The element count is irrelevant — `[8]`
is as good as `[1]` — so this is **not** a "the symbol is 8 bytes" story.

**What landed, and why the two files differ.** `src/dg_camera.c` takes
`extern struct dungeon *DUNGEON_PTR[2];` — `[]` and `[2]` are compatible types,
so none of its thirteen `DUNGEON_PTR[0]->` sites change.
`src/dungeon_map_access_1.c` takes the **scalar**, which does rewrite its two
existing use sites; that was measured byte-identical first
(`wip/overlay_29_02337EC0_all`, `probe/m.c`) and is confirmed by the build.
`[2]` is not a fiction: `DUNGEON_PTR` measures **8 bytes** — in
`asm/overlay_29_data_023534E0.s` the label is followed by two `.byte`
quadruples before the next `.global`. In-tree precedent for `[2]` predates this
commit at `src/special_move_types.c:17`, landed in
[`964beafb`](../commits/964beafb.md) for the same reason.

**Fact, census by grep after this commit:** 52 files declare
`*DUNGEON_PTR[]`, 31 declare the scalar, 2 declare `*DUNGEON_PTR[2]`. No header
in `include/` declares the symbol at all, so each file's spelling is local to
its translation unit — which is exactly why the disagreement is invisible to the
build and only a grep finds it. **Which spelling a file needs is decided per
function by what the allocator does with it**, so a tree-wide unification is not
free; it would need re-measuring each affected function.

**Suggestion, untested.** `docs/MATCHING_TIPS.md` frames this axis as
scalar-versus-array. `PopulateActiveMonsterPtrs`, which that entry records as
fixed by going scalar, would on this reading also be fixed by `[2]`. That is one
measurement nobody has run.

### Twenty-four call-site declarations retired; ten of them were wrong ([`967fe53b`](../commits/967fe53b.md), [`0f3cbcbc`](../commits/0f3cbcbc.md))

Five in `967fe53b` and **nineteen across fourteen files** in `0f3cbcbc` are
replaced by an `#include` of the header that now owns the function. **Ten were
wrong, not merely redundant** — facts, read off each callee's own asm:

*(This heading previously read "Twenty-one", and the sentence "sixteen across
twelve files", following `0f3cbcbc`'s commit message. Both were wrong for the
same reason the message was; the recount is after the commits table. The list of
which declarations were wrong is unaffected.)*

- **`PlayEffectAnimationEntity` in eight files**, every copy spelling the tail
  `(…, u8, s32, s32, s32)` (and disagreeing on the return: some `void`, some
  `s32`). Its prologue reads `u8`, `s16` and a **pointer** for the last three.
  It is now declared once, in `include/dg_effect.h`, as
  `s32 PlayEffectAnimationEntity(struct entity *, s32, s32, u8, s32, u8, s16, struct unk_0201C000 *)`.
  Every affected call site passes literals, so the correction is byte-neutral —
  confirmed by the build, not assumed. `struct unk_0201C000` is a **placeholder**
  and its members are inference.
- **`ov29_022FB984`** was declared `void`; it returns `bool8`.
- **`ov29_022FB98C`** was an unprototyped `extern int` for a function that
  returns nothing and takes two `struct entity *`.

This is the same failure mode the branch has hit repeatedly: **two declarations
in two translation units never meet, so the compiler cannot see them disagree
and the ROM matches either way.** Only a grep finds these.

### One asm file landed as two objects, because MWCC inlines a callee it has already seen (`967fe53b`)

**Fact.** `asm/main_02003328.s`' nine functions do not fit in one translation
unit. `GetTime` calls `EnableAllInterrupts`, which precedes it in address order,
and at `-O4,s` MWCC folds a callee whose body it has already compiled — in one
TU, `GetTime`'s two `bl EnableAllInterrupts` become the inlined body. So
`GetTime` and `DisableAllInterrupts` go to a new `src/main_020037B4.c` and the
other seven merge into `src/main_0200330C.c`; `main.lsf` keeps two objects where
it had one asm object and one C object. `DisableAllInterrupts` is not inlined in
the same run because it is *defined after* `GetTime` — the same rule seen from
the other side.

**The boundary is taste, and is recorded as such.** Any split point in
`(0x02003608, 0x020037B4]` removes the inline and produces the same bytes; no
measurement separates them.

### The parent-menu tables were typed from the weakest prototype that accepted them ([`92774e8a`](../commits/92774e8a.md))

**Fact.** `asm/overlay_11_022ECD24_data.s` defines `ov11_02322E00` as five 8-byte
entries — a `.word` string id followed by four bytes of value, terminated by
`00 00 00 00 / FF FF FF FF`. That is `struct unk_0202A5CC` (`u16`, 2 pad, `s32`),
which `include/main_0202A66C.h` already declares and which
`CreateParentMenuFromStringIds` already takes.

**Fact.** Before this commit `src/overlay_11_02307334.c` held, for the same shape:

* four tables declared `extern s32 ov11_02322D10;` — **a scalar for an array**,
  which is why every call site wrote `&ov11_02322D10`;
* `struct unk_02322D38` (`u16; u16; u32`), **byte-identical** to
  `struct unk_0202A5CC` and declared under a second name;
* its own `extern u8 CreateParentMenuFromStringIds(void *, u32, void *, void *);`,
  whose `void *` fourth parameter is what let all of the above compile.

`92774e8a` types all seven globals `struct unk_0202A5CC[]`, deletes the duplicate
struct, yields the prototype to the header, and drops five now-redundant `&`.
Byte-neutral: `&scalar` and an array decay to the same address.

**A reviewer should know how nearly this went the other way.** The two new tables
were first typed `u8[]` to fit the existing `void *` prototype. That version
**compiled under `-W error` and the US ROM matched** — because the difference is
a pointer *type*, which changes no address and no width. The build is
structurally unable to distinguish the two, so it would have shipped as a sixth
wrong declaration. Only the data settles it.

**Inference, flagged as such.** That these tables are *menus* rests on the
consumer's name and on the entries' `u16` looking like string ids. The layout is
fact; the meaning is not, and no name was introduced for it.

### An 809-instruction function closed by an allocator-ordering rule ([`92774e8a`](../commits/92774e8a.md))

**Fact, measured.** `ov11_023090DC` reached `STRUCT 0` — every instruction,
immediate and branch target correct, 820 rows against 820 — while a single
register permutation over 35 rows held it at score 230. Thirteen approaches were
falsified first and are listed in the wip's ledger, including a 120-permutation
sweep of function-scope declaration order.

**Fact, measured.** MWCC colours the locals that live across a call in
**case-scope declaration order**, lowest free register first, and a local
assigned only inside a loop body does not join that group from case scope. Four
qualifying locals therefore reach only `r4`–`r7`. The target needs one on `r8`,
so a **fifth member must exist** — which is why the earlier sweep found nothing.
Two independent routes then reached score 0.

**The transferable part**: the earlier ledger recorded "declaration order
EXHAUSTED" as a property of the *axis*. It was a property of the *set* — four
variables. A negative result on an ordering sweep is only as general as the set
swept, and should record which variables were in it.

### Three functions that do not exist in the JAPAN build ([`964beafb`](../commits/964beafb.md))

**Fact.** `ov29_022FBD08`, `ov29_022FBD24` and `ov29_022FBD80` sit inside one
`#ifndef JAPAN` in `asm/overlay_29_022FBC4C.s`. Their landed definitions *and*
their prototypes carry the same guard, so `check_landed_guards.py` reports three
definitions at conditional depth 1 for this commit — **correct here**, not the
defect that check normally catches.

### `struct dungeon` gained a sub-object to force one base register ([`964beafb`](../commits/964beafb.md))

**Fact.** `struct unk_022FBD24` groups the two members at `0x3DCC` and `0x3E1C`
into one sub-object: `u32[20]` then `u32`, same order, same offsets, so the
layout is byte-identical. **Inference, but strongly evidenced:** the grouping is
what makes MWCC materialise the single `add r3, r0, #0x3c00` base the target
uses; without the type `src/overlay_29_022FBBEC.c` does not compile at all. The
four comment lines moved verbatim with the members, which is the one mechanical
exception the no-comments rule allows.

### `struct damage_calc_diag` gained three padding bytes ([`38aa265d`](../commits/38aa265d.md))

The header modelled `move_type` as a 1-byte enum plus explicit
`field_0x1/0x2/0x3`, but gave `move_category` no padding, so under `-enum min`
every member from `move_indiv_type_matchups` (0x8) to `attacker_level` (0x16)
compiled below the offset its own comment states. Alignment before `damage_calc`
re-absorbed the drift, so the struct still totalled 0x54 and no build caught it.
`asm/overlay_29_022E0378.s` and `asm/overlay_29_022E335C.s` access both fields
with **word** instructions, so both are four bytes in retail.

Only two places in the tree use `last_damage_calc`, both past the
re-convergence point, so the change is byte-neutral — confirmed by a matching
build of all three regions. A reviewer may prefer to model both enums as
four-byte members and drop all six placeholders instead; that is byte-identical.
**Superseded for `move_category` by `76533508`, next.**

### `struct damage_calc_diag`: `move_category` widened to `s32`, the eight modifiers made `u8` ([`76533508`](../commits/76533508.md))

`CalcDamage` writes `move_category` with a 4-byte `str` (`0x0230BDC8`), which a
1-byte enum plus padding cannot produce (`strb`), so the field is now
`s32 move_category;` and the three placeholder bytes from `38aa265d` are gone —
the struct stays 0x54 bytes and the word-access evidence in the previous
section agrees. It also reads the eight modifier counts at 0x30-0x37 with
unsigned `ldrb` at all 24 sites, so they are `u8` where pmdsky-debug declares
`s8` (they are conceptually signed counts of −2..+2, which is presumably why).
No other source in the tree reads any of these members (grep), and the change
is confirmed by matching builds of all three regions. Both are deviations from
the upstream declarations and are worth raising there.

These are the changes that touch types shared with the rest of the tree. They are
the ones most likely to be contentious, and they are collected here so nobody has
to find them across 50 commits.

### `struct monster`: `bide_move_id` widened to a 2-byte `enum move_id` ([`d387d4e2`](../commits/d387d4e2.md))

The third deviation from pmdsky-debug's declarations on this branch, and the
same shape as the two `damage_calc_diag` ones above: upstream's byte is not the
width retail uses.

`0xAC` was `u8 bide_move_id;` followed by `u8 field_0xad;`. Retail accesses it
with `ldrh`/`strh` and stores **0x165 = 357 = `MOVE_BIDE_UNLEASH`**, which does
not fit in a byte. It is now `enum move_id bide_move_id;`, absorbing
`field_0xad`.

Why the enum rather than `u16`: `enum move_id` runs to `MOVE_TAG_0x22E = 558`,
so `-enum min` cannot size it below 16 bits, and the tree already depends on
that — `struct move` has `enum move_id id; // 0x2` sandwiched between two `u8`s
(`include/common.h:20-31`). It also means `InitMove`, which takes
`enum move_id`, is fed with no cast; a `u16` field would have needed one under
`-W error`.

The change is layout-neutral by measurement, not by argument: eight
compile-time assertions (including `unique_id` still at 0xB0 and
`sizeof(struct monster) == 0x240`) plus a full matching build with the change
alone, before any of the function that motivated it was written. No other source
in the tree reads `bide_move_id` or `field_0xad`.

**Worth raising upstream.** The field is conceptually a move id and pmdsky-debug
sizes it as a byte; codegen requires 16 bits.

### A stale extern retired, and a general jump-table fact ([`d814f8d0`](../commits/d814f8d0.md))

Two things a reviewer may want beyond the diff.

**`src/dungeon_projectile_throw.c` no longer declares `ApplyItemEffect`.** It
had its own `extern` plus a live call site; that is replaced by an include of
the new header. Worth noting because it is a landed, already-matching object and
parameters 1-3 are exactly where a wrong spelling would surface, so its rebuild
in all three regions is evidence about the signature rather than housekeeping.
Agreement, not proof — `param_1`'s `char` spelling remains undetermined from the
bytes.

**`case ITEM_NOTHING:` is written explicitly above `default:`**, even though the
jump table sends id 0 there. This was settled by surveying **all 786
`add<cc> pc, pc, rX, lsl #2` dispatches in `asm/`**: 104 of the 105 rebased
tables have a real case at index 0; rebases occur for offsets as small as 3, yet
40 tables have 3-7 leading default entries un-rebased; and 82 have trailing
default entries, which padding cannot explain. The table therefore spans the
minimum to maximum *labelled* case, and leading/trailing default entries are
explicit `case` labels sharing `default`'s body. That is a fact about MWCC, not
about this function, and it will matter for the next switch-heavy target.

### The first file split on this branch, and a struct that cannot yet be typed ([`f47593d9`](../commits/f47593d9.md))

Two things a reviewer should weigh beyond the diff.

**This is the branch's first `extract_function.py` SPLIT** rather than a merge —
the function sits mid-file, so 76 following functions move to a new
`asm/main_0203EFD4.s`, a fresh `src`/`include` pair is created, and `main.lsf`
gains two objects where it had one. Worth a look purely because the mechanics
differ from every other landing here.

**`struct unk_020AFE74` is a placeholder that deliberately does not claim
everything.** It models the 0x3C0-byte mission-reward state struct, but 427 of
those bytes — 0x1BF-0x2B4 and 0x303-0x3B7 — are **never touched by this
function**, confirmed by scanning every `[reg, #imm]` offset in the target. They
are `u8` filler, not inferred members. The other 88 functions in the object
exercise the rest, and the struct wants a second caller before it is typed
properly. Flagged because a filler range is exactly the sort of thing a later
contributor might "helpfully" name from one call site.

Also here, and cheap to check: `sub_02046C78` and `sub_02046D20` take **no**
arguments, contradicting `src/main_020663C8.c:3,5`. Two translation units never
meet, so the build cannot catch that — only a grep can.

### Three callee declarations corrected, two of which cancelled each other ([`240bea14`](../commits/240bea14.md))

`ov11_02307334` is the first caller that makes three long-standing declaration
errors matter, and all three are `-W error` blockers for it rather than style
points. Each is a **fact** read off the callee's own asm, and the matching build
confirms all three are byte-neutral.

| declaration | was | now |
|---|---|---|
| `RemoveItemNoHoleCheck` | `u32 (struct item *)` | `u32 (s16 index)` |
| `GetFirstUnequippedItemOfType` | `struct item *(s16)` | `s16 (s16)` |
| `ov10_022BCDA8` | `void (s32)` | `s32 (s32)` |

The first two are **one** error, not two, and this is the part worth a
reviewer's attention: `RemoveFirstUnequippedItemOfType` — the only other caller
of either — passes the second's return straight into the first's parameter.
Both were typed `struct item *` where the bytes say index, so the two mistakes
cancelled and the pair matched by accident. `SMULBB` multiplying by 6
(`sizeof(struct item)`) is what settles it.

`ov10_022BCDA8`'s body ends in `sub_02033064`, which returns `s32`, so `r0` is
live at return; this function consumes it twice.
`src/overlay_10_022BCC60.c` gained the matching `return`.

### The asm's region forks that the C does not need ([`240bea14`](../commits/240bea14.md))

`asm/overlay_11_022FE5F8.s` carries four `#ifdef JAPAN` **code** forks inside
this function, and the C reproduces all four with **no `#if` at all** — three
file-scope offset macros and two genuine three-way `EUROPE` blocks cover the
whole function, 11 preprocessor lines against the asm's 11 directive groups.

The reason is mechanical: an id gets a code fork **iff exactly one** of its two
values is an ARM rotated-8-bit immediate, so US/EU can use `mov` where JP needs
a pool word. Writing `id + OFFSET` and letting MWCC choose reproduces both arms.
This was predicted as inference before the JP build, with the two forks that
also *reorder* three instructions named as the likeliest failures — they were
not.

Worth knowing for the next region pass: the `+0x2D20` family has 15 members but
the asm writes its macro on only 11 pool words; the other four are hard-forked
for encoding reasons and carry the same shift. Applying an offset macro to an
id from a *different* family would produce a wrong JP ROM that **the US and EU
builds still match**, so only a JP build can catch it.

### A parameter type only a caller can see: `SetActionUseMovePlayer`

Landed as `(struct action_data *, u8, u8)` and corrected to `(…, s32, s16)`
([`0587a6ef`](../commits/0587a6ef.md)). The callee stores both parameters with
`strb`, so `u8`, `s16` and `s32` all match its own asm; only `SetLeaderAction`'s
call site — which passes one argument unchanged and sign-extends the other to 16
bits — distinguishes them. The same failure mode as
`CanMonsterMoveInDirection`'s `u16` parameter. **A parameter whose only use is a
narrowing store cannot be typed from the callee alone.**

### The stat-index globals are now `const`

`ATK_STAT_IDX` and `SPATK_STAT_IDX` are declared `const` in all thirteen `src/`
files that reference them ([`1bcd9852`](../commits/1bcd9852.md)). This is what
makes eight stat-helper wrappers match: a call with a stack argument stores to
memory, and a non-`const` global's load cannot hoist above those stores. Every
previously-matching caller still matches after the change.

### A stale extern replaced: `BoostDefensiveStat` and the stat-index globals

`src/overlay_29_0232E250.c` declared `ATK_STAT_IDX` / `SPATK_STAT_IDX` as `s32`
and `BoostDefensiveStat` with an `(s32, s16)` tail, all contradicting
`include/move_orb_effects.h`, where the stat is a one-`int` `struct StatIndex`
passed by value ([`ba4d07a4`](../commits/ba4d07a4.md)). The declarations lived in
different translation units, so the compiler never saw the conflict and the ROM
matched regardless. Replaced with an include of the canonical header.

### A shared prototype widened: `CanMonsterMoveInDirection`

`bool8 CanMonsterMoveInDirection(struct entity *, u16 direction)` becomes `s32
direction` ([`e8823a88`](../commits/e8823a88.md)), in both the header and the
already-matching definition in `src/dungeon_capabilities_3.c`.

**Why:** a `u16` parameter makes the caller narrow the argument
(`lsl #16; lsr #16`). At `SetLeaderAction`'s call site retail passes the value
unchanged. Any word type matches; `enum direction_id` does not.

**Weigh it:** the definition matches either way, and the only other C caller
passes a `u8` field, so neither can distinguish the two. This is the first call
site that can, and it says word. It is still a divergence from pmdsky-debug's
`u16`.

### A struct field pair retyped: `dungeon.field_0x1d8` / `field_0x1dc`

Four `u16` fields become two `struct position`s
([`e8823a88`](../commits/e8823a88.md)). Same size, same alignment, and no other
C code touches them. `dungeon.field_0x614` also becomes `s32` (tested `>= 0`),
and `display_data.leader_target_direction_mirror` becomes `u8` — the enum type
folds the stored `0xFF` to `-1` and drops an instruction retail has.

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

**There are now three.** `967fe53b` added `ITEM_INVALID = -1` to `enum item_id`
on the same reasoning and to the same standard of proof — see the section at the
top of this list. This heading is left at "two" because it is the record of what
those two commits did; the count for the branch as a whole is three.

### `struct bag_items` extended three times — now four

- [`27c9ec9a`](../commits/27c9ec9a.md) — four fields appended past `maybeMoney`.
- [`35134ece`](../commits/35134ece.md) — four more, up to `0x13B4`.
- [`152f6c7e`](../commits/152f6c7e.md) — **filler split into three runs** to
  expose two pointers at `0x132C` and `0x1370`.
- [`14339589`](../commits/14339589.md) — **the three filler runs replaced
  outright** by the members behind them. Added later; see the full section at the
  top of this list.

The first two only append, and cannot move an existing member. **The third
genuinely could have**, and depended on the rebuild of all four translation units
that use the struct to prove `maybeMoney` still lands at `0x1394`. The fourth is
the largest by far and rests on the same rebuild plus an unchanged `sizeof` of
`0x13B4`; a reviewer weighing the third should weigh it together with the fourth,
since they touch the same bytes and the fourth supersedes the third's filler
split.

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

### Three field types corrected, and a message-id parameter (`eb0db73b`)

`ApplyDamage` required `struct monster::bide_damage_tally` `u32` -> `s32`
(clamped with `strgt`), `struct monster::field_0x168`/`field_0x169` merged into
one `s16 field_0x168` (read `ldrsh`), and
`struct dungeon_generation_info::music_table_idx` `u16` -> `s16` (read `ldrsh`).
Each is forced by a load or store width in the target.

The same commit changes the first parameter of
`TalkToSecretBazaarNpcStandard`, `ov29_022F0618`,
`TalkToSecretBazaarNpcWithYesNoMenu` and the `TalkToSecretBazaarNpc` extern from
`struct entity *` to `s32`. Every caller loads a small constant into `r0`
(`0xC6B` from ApplyDamage; `0xF32`/`0xF4A`/`0xF4B`/`0xF4C` in
`asm/overlay_29_02344178.s`), and the three tree functions are one-line
forwarders whose parameter types their own bodies never constrained. This is the
same failure mode as `SetActionUseMovePlayer` above: **a parameter that a
forwarder only passes through cannot be typed from the forwarder.**

### A deliberate declaration divergence: `DUNGEON_PTR` (`eb0db73b`)

`src/overlay_29_02308FBC.c` declares `extern struct dungeon *DUNGEON_PTR;`
where `src/dg_camera.c`, `src/dg_uty.c` and `src/dungeon_ai.c` declare
`extern struct dungeon *DUNGEON_PTR[];` and index `[0]`. The array form lets
MWCC CSE the pointer load, which costs three instructions the target has.

**No build can catch this** — two declarations in two translation units never
meet. It is called out here and in the commit note because only a grep finds it.
The scalar form is arguably the more honest declaration for a single pointer,
and `src/dungeon_ai_items.c` already spells `BAG_ITEMS_PTR_MIRROR` scalar, but a
reviewer may want the tree unified one way or the other.

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
| `DUNGEON_PTR` (data) | `overlay_29_02308FBC.c` declares it scalar; `dg_uty.c`, `dungeon_ai.c` and 55 other files declare `*DUNGEON_PTR[]`; `dg_camera.c`, `special_move_types.c`, `overlay_29_022E9FC0.c` and `overlay_29_023047B8.c` declare `*DUNGEON_PTR[2]`, and `dungeon_map_access_1.c` uses the scalar — **three spellings across 92 files: 57 / 31 / 4** as of `c482e2df`, counted by grep (it was 52 / 31 / 2 at `0f3cbcbc`) | **deliberate, per function** — the lever is complete-vs-incomplete declared type rather than array-vs-scalar, measured in the sections above ([`eb0db73b`](../commits/eb0db73b.md), `0f3cbcbc`, `637d1966`, `14339589`) |
| `DrawTextInWindow` | **five** declarations that disagree, in no header. [`9b3650fe`](../commits/9b3650fe.md) landed the definition (`s32 window_id`) and replaced four of them with the header | **The fifth must stay.** `overlay_13_0238BDA8.c`'s `DrawPersonalityTestDebug` holds the id in an `s8` local, so against the header's `s32` the call gains a sign extension retail does not emit — replacing it broke `OVY_13.sbin`. Retail's TUs genuinely disagreed here; the per-TU declaration is evidence, not untidiness |
| `CreateSimpleMenuFromStringIds` | 3rd parameter typed `s32` in `overlay_25_init.c:38` and `main_0203D538.c:75` | **genuinely wrong** — it is a pointer (`add r2, r1, #0x1c` at [`240bea14`](../commits/240bea14.md)'s call site). Every earlier call site passes a literal `0`, so nobody had exercised it; that file carries a cast until the prototype is fixed |
| `CloseTextBox2` | `overlay_25_init.c:9` declares `(s8)`; `overlay_31_02383880.c:16` declares `()` and calls it with **zero** arguments | the callee reads `r0`. Whether those zero-argument calls still match was not established ([`240bea14`](../commits/240bea14.md) declares one parameter) |

The `overlay_25_init.c` `UpdateWindow`/`sub_02027B1C` case and the
`CreateSimpleMenuFromStringIds` row are the *incorrect* ones. Fixing it properly
means retyping `ov25_0238B414`'s own parameter and its callers, which is its own
piece of work.

**Closed since:** `DeleteWindow`'s provisional declaration in
`include/main_0202AAA8.h` was replaced by an include when the function landed in
[`702c4c85`](../commits/702c4c85.md), with all three decompiled callers rebuilt.
**`PlayEffectAnimationEntity`'s eight mutually-agreeing-but-wrong declarations**
were replaced by `include/dg_effect.h` in `0f3cbcbc` (see the section above), and
`ov29_022FB984` / `ov29_022FB98C`'s wrong declarations by
`include/dungeon_logic_4.h` in `967fe53b`.

### A construct that is a stand-in, not recovered source

[`eb0db73b`](../commits/eb0db73b.md) matches `ApplyDamage` with one `volatile`
read:

```c
EndCurseClassStatus(defender, defender,
                    *(volatile u8 *)&dmon->curse_class_status.curse, 0);
```

Retail compares that byte, branches, and loads it again to pass it, with no
call or store in between. Without the `volatile`, MWCC allocates the value once
and emits one instruction fewer. It is standard C, no inline asm is involved,
and the ROM matches — but `volatile` on a game-data field is not plausible
source, so this line is **evidence of the mechanism rather than recovered
code**.

The closest natural form, a `struct curse_class_status *` local, reaches
1623/1624 rows and differs in exactly one instruction. If a better construct
turns up it should replace this line verbatim.

[`76533508`](../commits/76533508.md) matches `CalcDamage`'s ability-multiply
block with an `s32 calc[2]` local, two temps and three `volatile s32 *`
pointers to it. Every element removes one measured compiler obstacle (the
note lists them: address-expression propagation, the scheduler's store
ordering, the allocator's simplify order); it is ordinary C and compiles to
the bytes, but it is **match-derived, not recovered source**. The function
was byte-identical outside those eleven instructions from early on; a plainer
spelling that satisfies the same conditions would be welcome and should
replace it verbatim.

`967fe53b` adds a third `volatile`, and it is the one case on this branch where
the construct is **plausible source rather than only a codegen lever**.
`include/main_0200330C.h` declares `volatile s32 field_0x1c;` and
`volatile s32 field_0x20;` in `struct unk_020AEF7C`. *Fact*: the qualifier is
forced by the output — without it MWCC folds `GetTime`'s two reads of
`field_0x1c` into one (the target loads it twice with no call between), and
sinks `sub_02003704`'s `field_0x20` increment above the `OS_IRQTable`
read-modify-write. *Inference, but well supported*: both members are incremented
from interrupt/alarm context (`sub_02003754` and `sub_02003704` are what write
them), which is what `volatile` is for, so unlike the two stand-ins above this
one is what the original would plausibly have carried. Two neighbouring counters
are **not** marked, because nothing measures them; `field_0x34` is
demonstrably not volatile.

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
3. **`build-tools/build.sh` could not express a deletion** — it copies changed
   files into a clean clone, but `extract_function.py` *removes* an emptied
   `asm/*.s` and `common.mk` globs `asm/*.s`, so the stale file returned and was
   assembled. Found in [`7c02eea6`](../commits/7c02eea6.md); **fixed** during
   [`dada4f8d`](../commits/dada4f8d.md) — an absent path is now removed from the
   clone, so `git status --short` output can be passed wholesale.
4. **`extract_function.py` fails with `Start line None`** on a signature written
   `struct item *Foo(...)` — it parses the name as `*Foo`. Write
   `struct item* Foo(...)`.

5. **`tools/m2ctx/m2ctx.sh` passes `-include global.h`**, which does not exist in
   this repo (it is `include/global.pch`). It does not fail loudly — it emits a
   12-line context of predefined macros only. Found in
   [`e8823a88`](../commits/e8823a88.md); substituting `include/global.pch` works.

6. **`extract_function.py` leaves an orphan `#endif`** when the extracted
   function is the **first** in its `.s` *and* sits inside a region conditional:
   the tool takes the opening `#ifdef` with the function and leaves the closing
   directive behind in the remainder file. Found in
   [`c482e2df`](../commits/c482e2df.md) extracting `ov29_022E34A8_EU` from
   `asm/overlay_29_022E34A8.s`; **removed by hand in that commit, and the tool
   was not changed** — so the next region-guarded first-function extraction will
   hit it again. *Inference, not checked here:* unlike defects 1-3 and 5 this one
   should fail loudly, since an unbalanced `#endif` is a preprocessor error — but
   nobody recorded building the unrepaired file, so do not rely on that.

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
- **Should `DUNGEON_PTR` be a scalar or an array tree-wide?**
  `SetLeaderAction` needs the scalar spelling; three other files use the array
  form. They are not byte-interchangeable at that call density, and there is no
  header declaration to settle it.
- **Two `volatile` stand-ins remain in `SetLeaderAction`** and are almost
  certainly not what the original source said — they encode a no-CSE property,
  not recovered code.
- **Two species ids in `IsMonsterAffectedByGravelyrockGroundMode` are left raw**
  ([`152f6c7e`](../commits/152f6c7e.md)) and are almost certainly named
  enumerators.
- **`struct damage_calc_diag` now disagrees with pmdsky-debug twice**
  ([`76533508`](../commits/76533508.md)): `s32 move_category` where upstream
  has a 1-byte enum, and `u8` modifier counts where upstream has `s8`. The
  bytes force both; the question is whether to change upstream's declarations
  or carry the deviation.
- **The JP build of `CalcDamage` compares the attacker's `apparent_id`** where
  US/EU compare the defender's, at the defense-side `0x211`/`0x218` checks.
  The bytes force the `#ifdef JAPAN`; whether it is a known regional bug is
  not recorded anywhere we found.
- **`CalcDamage`'s ability-multiply block is match-derived C** (see *A
  construct that is a stand-in*); the parameter names for its arguments 7-9
  are inferred, `a9` is a placeholder.

### Gathered from `967fe53b` and `0f3cbcbc`

- **There are now three `-1` sentinels in shared enums**, not two:
  `ITEM_INVALID` joins `ENTITY_NONE` and `MONSTER_INVALID`. The policy question
  at the top of this list — whether they belong upstream at all — now covers
  `enum item_id` as well. Each was verified byte-neutral in isolation; none of
  that answers the policy question.
- **Should `struct item::id` be `enum item_id` rather than `s16`?** It would
  delete the `(enum item_id)` cast that `-W error` forced into
  `AuraBowIsActive`. **Untested, and not a recommendation** — `struct item` is
  used everywhere, and `struct item_volatile` is a parallel declaration of the
  same layout that would have to move with it.
- **Is `struct unk_02337EE8` an object the original source had, or only the
  spelling that reproduces the arithmetic?** The bytes do not distinguish it
  from six other spellings that carry a cast. The same question stands for
  `struct unk_022FBD24` from [`964beafb`](../commits/964beafb.md); both are
  grouping-for-codegen, and a reviewer may want a single answer for both.
- **Two comments in `include/dungeon.h` need a human.** `speed_boost_counter`'s
  comment went with the member it documented — the tree's only record of what
  `0x1F` is — and `number_completed_floors`' surviving comment still doubts a
  width that has since changed. Neither was touched, because Claude does not
  author comments in `pmd-sky`.
- **Does the tree want one `DUNGEON_PTR` spelling?** It now has three (52 `[]`,
  31 scalar, 2 `[2]` **as of `0f3cbcbc`; 57 / 31 / 4 as of `c482e2df`** — see the
  updated census in the cross-cutting section). Unifying it is not a sweep:
  `ov29_022E34C8` and
  `ov29_0233804C` match **only** under a complete type, the earlier entry above
  (*A deliberate declaration divergence*, `eb0db73b`) records `SetLeaderAction`
  needing the scalar where its neighbours carry the array form, and the 52 files
  still on `[]` have not been measured under
  anything else. Every change of spelling is a per-function re-measurement. This
  extends, rather than replaces, the earlier `DUNGEON_PTR` question above.
- **Is `src/main_020037B4.c` the right file boundary?** Any split point in
  `(0x02003608, 0x020037B4]` removes the `EnableAllInterrupts` inline and
  produces the same bytes; `sub_02003620` would work too and would give a larger
  second file. Nothing measures the difference, so it is taste — recorded rather
  than hidden.
- **Two byte-identical alternatives in `967fe53b` that were not shipped**, both
  one-line swaps if the PR prefers them. (1) `_020AEFB4` / `_020AEFC8` are
  almost certainly members of the object at `_020AEF7C` (`0x38 + 0x14 + 0x14 =
  0x60`, and `0x020AEF7C + 0x60 = 0x020AEFDC` exactly; neither label is
  referenced anywhere else in the tree). Writing them that way is identical in
  the ROM but **cannot score 0 on a scratch**, because the pool word then renders
  `.word _020AEF7C+0x38` against the target's `.word _020AEFB4` — the scoreable
  form was shipped. (2) `*(vu32 *)HW_INTR_CHECK_BUF |= 1;` is the real SDK
  spelling of `sub_02003704`'s first line and was measured to emit an identical
  instruction stream, differing only in the pool-word *symbol*.
- **`struct unk_0229B220` is `u8[0x28]`** — the size is measured and
  `OS_SetPeriodicVAlarm` writes through `+0x24`, but the layout was not worked
  out because nothing in the group reads it. It is the SDK's `OSVAlarm`, and
  this tree has no VAlarm header.
- **`struct unk_0201C000` is a placeholder for `PlayEffectAnimationEntity`'s
  eighth parameter**, and its members are inference; every decompiled call site
  passes a literal for it, so nothing there constrains the layout.
- **`u8 (*field_0x10)(void)` in `struct unk_020AEF7C`**: the return width is a
  fact (`blx sl; strb r0, [r4, #6]`), the *empty* parameter list is not —
  nothing in the group passes an argument, but a callee could take one.

### Gathered from `6f3ad694`, `637d1966`, `14339589` and `c482e2df`

Collected from each batch's own `wip/*/STATUS.md`, which are contemporaneous
records; where a batch's STATUS has no open-questions section, nothing is
invented for it.

- **Which object at `0x022AB918` is really `TEAM_NAME`?** The two-symbol model is
  forced by the literal pool; the *assignment* of the existing name to the outer
  record rather than to the `0xC`-byte name buffer is not determined by anything
  measured, and pmdsky-debug has no symbol in that range to settle it. The
  batch's own note calls this **the single thing in it most worth a reviewer's
  push-back** (`wip/main_020504BC_all/STATUS.md`).
- **`u8 GetRank(void)` contradicts the in-tree `extern s32 GetRank(void)`.** The
  unsigned `movhs`/`movhi`/`blo` comparisons in `GetRank`, `GetRankupPoints` and
  `sub_02050CD0` are the evidence, and `u8` scores 0 in all three; only a human
  can say whether `src/main_0203D538.c`'s call site should be updated in the same
  change.
- **`DUNGEON_FRAMES_PASSED` is declared two ways in the tree** — `u32`
  (`src/overlay_29_022E869C.c:11`) and `struct unk_0237C850`
  (`src/overlay_29_022E9FC0.c:55`). `TryWarp` reads one word and masks it, so
  `u32` reproduces the bytes; if the struct is right, the correct spelling is
  `DUNGEON_FRAMES_PASSED.<field>`. **A pre-existing disagreement, not introduced
  by these commits, and not resolved by them.** Related and also open: whether
  `DUNGEON_FRAMES_PASSED` is really *one* object — the bss splitter emits a
  `.global` at every address a literal pool names, so the separate `ov29_0237C864`
  label is not evidence either way; the evidence *for* one object is that
  `overlay_29_022EA008` reaches `0x34` from the single pool word. The
  counter-evidence would be another function using `ov29_0237C864` as a base with
  its own small displacements, which the still-asm `asm/overlay_29_022ED888.s` and
  `asm/overlay_29_023456BC.s` may supply.
- **`ov29_023529B8` is in `.rodata` but is declared non-`const`.** `const` is a
  known CSE lever in this toolchain, so it was left as measured rather than
  changed untested. Someone who wants the more accurate declaration should
  **measure** it, not assume it is free.
- **Is `struct bulk_item` in the right place in `include/item.h`?** It has to sit
  above `struct bag_items` now that it is a member type; a forward-declared
  pointer plus a separate array typedef is the alternative that was not pursued.
  And the outer bound `2` on `field_0x1330` / `field_0x1374` rests on `cmp r2, #2`
  in `AllKecleonShopsZInit` alone.
- **The two `GetCurrentKecleonShop*` accessors now return `struct bulk_item *`**,
  which touches four files outside their batch. Byte-neutrality is measured;
  reverting to `u32 *` costs one cast in `SetActiveKecleonShop` and freezes a
  declaration the asm shows is wrong. A reviewer may still prefer the smaller
  blast radius.
- **`struct unk_02353554`'s internal boundaries are invented.** Only the
  referenced offsets and the `0x230` total are measured; seven members are padding
  whose lengths were chosen, and the `field_0x80[0x38]` / `field_0xb8[0x170]`
  split is a guess at where one sub-object ends. Related: `struct unk_022E8054`'s
  first `0x10` bytes are almost certainly four function pointers, but that is
  inference from `AssignTopScreenHandlers`, which is still asm — when it lands,
  that type should probably be rewritten and embedded at `0x14`, at which point
  `field_0x24` disappears into it. It is also named for the wrong function by the
  placeholder rule's lowest-address tie-break; `AssignTopScreenHandlers` is
  arguably the better namesake.
- **Two presentation calls in `6f3ad694` that an anonymous union would remove.**
  `struct unk_0202AAA8`'s `0x1B0` is read both as a `u8` and written as a word,
  and `CreateSimpleMenuInternal` reaches its `struct struct_2` argument through
  `*(struct struct_2 *)&menu->field_0x100`. An anonymous union — a construct the
  tree already uses in `include/preprocessString.h` — would remove both, at the
  cost of inventing a second field name for one offset, which the naming rules do
  not cover. Both spellings compile to the same bytes; this was left for a human
  rather than decided unilaterally.
- **`ov29_022ED800`'s first argument is `dungeon != NULL ? &dungeon->gen_info :
  NULL`**, which matches byte-for-byte but is an odd thing for a human to write —
  especially as the third argument `&dungeon->display_data` is computed
  **unconditionally** from the same possibly-NULL pointer. An in-tree
  `static inline` accessor would be more plausible source; none was found, and the
  bytes do not distinguish them. Same file: `UnkMapRelatedFunc`'s `case 999:` is
  read as an *empty* far case (a comparison whose result is discarded), and cases
  10 and 12 as absent from the source entirely — both are inferences from the
  table's shape, not from any semantic.
- **`struct ground_monster`'s five retyped members deviate from pmdsky-debug**
  (`u8` where upstream has `s8`, `s16` where it has `u16`), and the retype pins
  `ApplyGummiBoostsGroundMode`'s second parameter to `s16 *` while that function
  is still asm. Worth raising upstream alongside the `damage_calc_diag` and
  `bide_move_id` deviations rather than separately — the branch now carries
  **several** independent pmdsky-debug declaration disagreements, all of the same
  shape (upstream's width or signedness is not the one retail's loads use), and
  they would be better argued as one case than one at a time. *No exact count is
  given here because nobody has enumerated them across all 131 commits.*
- **Three field comments have now been deleted by width or grouping changes** —
  `speed_boost_counter`'s (`0f3cbcbc`), `walk_anim_frames_left`'s and `pos`'s
  (`14339589`), and `bitstream::ptr`'s *"Current byte"* (`c482e2df`) — plus
  `portrait_params`' "/ 8" scaling note. None was replaced, because Claude does
  not author comments in `pmd-sky`. If a reviewer wants the documentation kept,
  this is the systematic cost of the retyping work and it needs a human pass, not
  a per-commit fix.
- **Does the `sub_020559D8` result change how near-matches should be handled?**
  It was landed as its own asm object at score 75 and closed one commit later by
  a one-line respelling in a statement that appeared in no differing row. The
  split was correct by the branch's own standard, but it cost an object create
  and delete in consecutive commits — a reviewer reading either commit alone sees
  only half of it.
