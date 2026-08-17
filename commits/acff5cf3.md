# `acff5cf3` — Decomp the generic-LFO and note-random-region track events

| | |
|---|---|
| **Commit** | `acff5cf3` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `909484fb` |
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
| `DseTrackEvent_SetNoteRandomRegion` | `0x02072144` |
| `DseTrackEvent_SetupLfoEnvelope` | `0x020727C8` |
| `DseTrackEvent_UseLfo` | `0x02072938` |

**Fact.** `SetNoteRandomRegion` reads two bytes and stores the **smaller** in
`note_random_region_begin` and the **larger** in `note_random_region_end`, so the
pair is sorted regardless of the order it was written in.

**Fact.** `SetupLfoEnvelope` and `UseLfo` are the **indexed** forms of the
per-slot handlers in [`909484fb`](909484fb.md): instead of a fixed
`lfo_settings` entry they take the index from `channel + 0x61`, which `UseLfo`
also writes.

## A field named only by its offset, on purpose

**Fact.** That byte falls inside `dse.h`'s `field_0x5A[10]`, so it is spelled
`field_0x5A[7]`.

**Deliberate refusal.** A *name* for it would have to come from a real
pmdsky-debug sync, not from this branch inventing one. The offset arithmetic is
checkable; a name would not be.

## The finding: bind the sub-struct to a pointer local

**Fact.** Indexing `channel->lfo_settings[idx]` directly **folds the array's
`0x74` base into each store's offset**, giving `strb [r1, #0x75]`. The target
computes the element address once -- `add r1, r3, #0x74` then
`add r1, r1, idx lsl #4` -- and stores at `[r1, #1]` and `[r1, #2]`.

**Fact.** Binding `struct dse_lfo_settings *lfo` also reproduces the **frame
push** the target has, which no arrangement of the indexed form did.

This is the same base-vs-folded-offset mechanism that recurs throughout the
branch -- see [`7f6977e2`](7f6977e2.md), where it is stated as a general rule
about literal pools.

## Naming

Nothing new.
