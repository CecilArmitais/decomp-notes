# `f7fbcd80` — Decomp six collection-menu setters; add a second window-contents view

| | |
|---|---|
| **Commit** | `f7fbcd80` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `ec362491` |
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
| `SetCollectionMenuField0x1BC` | `0x0202C5E0` |
| `SetCollectionMenuField0x1C8` | `0x0202C794` |
| `SetCollectionMenuField0x1A0` | `0x0202C7A8` |
| `SetCollectionMenuField0x1A4` | `0x0202C7BC` |
| `SetCollectionMenuVoidFn` | `0x0202C7D0` |
| `SetCollectionMenuField0x1B2` | `0x0202D0D8` |

## The decision worth reviewing: two views, not one struct

**Fact.** These reach the same buffer as the parent, simple and advanced menu
accessors, through the same `GetWindowContents` -- **but they do not share its
layout**:

- this set writes a **word at `0x1A0`** where `CheckParentMenuField0x1A0` reads a
  **byte** there;
- it writes a **byte at `0x1B2`**, which falls **inside the word** the advanced
  text box uses at `0x1B0`.

**So they are given their own view, `struct unk_0202C5E0`**, named for the
lowest-addressed function that takes it, rather than forcing one struct to
describe both.

**The justification, and it is a good one.** `GetWindowContents` returns
`void *`, so each menu type casting the buffer to its own layout is consistent
with what the target does. **A single merged struct would have to assert that the
two layouts agree, which the stores show they do not.**

This is a genuine design call a reviewer may want to weigh: the alternative is a
union, or a tagged discriminated layout. Two independent views was chosen as the
option that asserts least.

## An inference labelled at its actual strength

**Fact, and explicitly weak.** Two of the offsets are inferred **from the store
width alone and nothing else**. `0x1A8` is written with a word and the function
is named `VoidFn`, so it is typed `void *` -- **but the target would look the
same for any pointer or 32-bit value.**

## Naming

Nothing new beyond placeholders.
