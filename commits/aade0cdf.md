# `aade0cdf` — Decomp GetWindow; retype overlay_31's window callbacks as ids

| | |
|---|---|
| **Commit** | `aade0cdf` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `2472ad54` |
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
| `GetWindow` | `0x020275F8` |

**Fact.** It indexes `WINDOW_LIST` with a stride of `0xE0`, so its argument is a
**window id rather than a pointer**, and its result is the address of one entry.

## Correcting a wrong interface that was already in the tree

**Fact, and the argument is decisive.** `overlay_31_02382820.c` declared it as
taking a `struct Window *` and **passed one**, which cannot be what the target
computes: **multiplying a pointer by `0xE0` and adding it to `WINDOW_LIST` is
meaningless.**

**Inference, but forced.** The value it passes is an id, and the *same* value
goes to `DrawTextInWindow` and `UpdateWindow`, so those take ids too. This
retypes the three window callbacks and both prototypes accordingly, along with
the callback type `CreateTextBox` stores.

**Fact, verified rather than assumed.** The retyping is byte-neutral -- a pointer
and an id both travel in `r0` -- but `overlay_31_02382820.o` was **already
matching**, so it is rebuilt here to confirm.

## A correction to an overstatement made during this work

While reviewing this, the problem was initially described as `GetWindow`'s
declaration being wrong in both directions. **That was too strong.** The
*return* type `struct Window *` was fine, and the 7-byte struct was merely
**incomplete**, not incorrect -- an index into an incomplete type still resolves.
**Only the parameter was actually wrong.**

Recorded because the note should not preserve an exaggeration that was corrected
in conversation.

## Struct placement

**Fact.** `struct Window` keeps its existing return type and gains padding to its
real `0xE0` size, which is what lets `&WINDOW_LIST[id]` stride correctly. **Its
first seven bytes are unchanged**, so the `width` field overlay_31 reads is where
it was.

**Fact.** The struct moves from the overlay's header into `include/window.h`,
which already exists and is where a main-binary function returning one can reach
it -- **the overlay includes it, rather than the main binary including an overlay
header.** That direction matters.

## Open questions

- The `0xE0` struct is padding at this point. It is replaced with a real layout
  two commits later in [`30ea1f34`](30ea1f34.md).
