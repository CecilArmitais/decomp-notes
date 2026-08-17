# `fdbc544f` — Decomp GetNbItemsInBag and IsItemInBag

| | |
|---|---|
| **Commit** | `fdbc544f` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `399618a4` |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK` |
| **Note written** | **retroactively**, from the commit message and the diff |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The diff and the matching build are the sources of
> truth — see [the README](../README.md). Claims are labelled **fact** or
> **inference**.
>
> **This note was backfilled, not written at the time.** It is reconstructed from
> the commit message and the diff, both of which are reliable; but the dead ends,
> intermediate scores and discarded candidates a contemporaneous note would
> record are **lost**, and nothing here reconstructs them by guess. Where this
> note is silent on what was tried, read that as "not recorded" — never as
> "nothing was tried".

## Decompiled

| function | address |
|---|---|
| `GetNbItemsInBag` | `0x0200EDFC` |
| `IsItemInBag` | `0x0200EEE0` |

## Two techniques that recur through the whole bag cluster

**1. Cursor walk, with both increments in the `for` clause.**

**Fact.** Both walk the active inventory with a **cursor rather than indexing**,
with the index and the cursor **both advanced in the `for` clause** -- which is
what puts the two adds at the top of the loop body, as the target has them.

**2. Boolify the flag test through an explicit `bool8`.**

**Fact.** `GetNbItemsInBag`'s existence test goes through an explicit boolean:
`(item->flags & ITEM_FLAG_EXISTS) != 0` assigned to a `bool8` before the branch.
That is what produces the target's `tst` / `movne` / `moveq` / `tst #0xff`
sequence. **Testing the masked value directly collapses it.**

The inverse case appears in [`06c828e1`](06c828e1.md), where a *bare* mask is
correct -- so the two forms are not interchangeable and the target distinguishes
them. The tell is whether the emitted sequence boolifies before branching.

**Fact.** `IsItemInBag` returns early on the first match, which the target
reaches with `bxeq lr` mid-loop, and falls through to a single `return 0`.

## File placement

`GetNbItemsInBag` lands in the existing `src/main_0200EDC0.c`, which already
includes `item.h`. `IsItemInBag` needs a new file and declares
`BAG_ITEMS_PTR_MIRROR` alongside it, **matching how the other bag sources in this
tree declare it** rather than introducing a new spelling.

## Naming

Nothing new.
