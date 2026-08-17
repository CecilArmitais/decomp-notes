# `a49bcedf` — Decomp CountNbItemsOfTypeInBag and HasStorableItems

| | |
|---|---|
| **Commit** | `a49bcedf` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `3c5de72d` |
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
| `CountNbItemsOfTypeInBag` | `0x0200EE4C` |
| `HasStorableItems` | `0x0200F0FC` |

## A behaviour that looks like a bug in the decomp and is not

**Fact, and flagged deliberately.** `CountNbItemsOfTypeInBag` counts every slot
whose id matches **without checking the existence flag**, so empty slots are
counted when the caller asks for whatever id they hold.

**That is what the target does; it is not an omission here.** Recording this
matters, because the natural review reaction is "you forgot the existence check"
-- and a future contributor "fixing" it would break the match.

**Fact.** `HasStorableItems` checks the existence flag through the same explicit
`bool8` the other bag walkers use (see [`fdbc544f`](fdbc544f.md)), then calls
`IsStorableItem`, already decompiled in `item_util_4.c`, and returns on the first
slot satisfying both.

## An insertion mechanic worth knowing

**Fact.** `HasStorableItems` sits at a **lower address** than `GetItemIndex`, so
`extract_function.py` **prepends** it to that file. The `extern` for
`BAG_ITEMS_PTR_MIRROR` was below the existing function and had to be **moved to
the top**, above both, since the new first function needs it too.

This failure mode -- a merged function landing above an existing `extern` -- is
now in `docs/INSERTING_AND_BUILDING.md`.

## Naming

Nothing new.
