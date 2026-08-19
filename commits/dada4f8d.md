# `dada4f8d` — Decomp ten more move-effect wrappers in overlay_29

| | |
|---|---|
| **Commit** | `dada4f8d` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `7c02eea6` |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK` |
| **Note written** | with the commit |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The diff and the matching build are the sources of
> truth — see [the README](../README.md). Claims are labelled **fact** or
> **inference**.

## Decompiled

**Fact.** Fourth batch of the same template:

| function | forwards to | constant arguments |
|---|---|---|
| `DoMoveWrap` | `TryInflictWrappedStatus` | — |
| `DoMoveMagicCoat` | `TryInflictMagicCoatStatus` | — |
| `DoMoveProtect` | `TryInflictProtectStatus` | — |
| `DoMoveDestinyBond` | `TryInflictDestinyBondStatus` | — |
| `DoMoveMirrorCoat` | `TryInflictMirrorCoatStatus` | — |
| `DoMoveSnatch` | `TryInflictSnatchStatus` | — |
| `DoMoveReflect` | `TryInflictReflectStatus` | — |
| `DoMoveSeeTrap` | `RevealTrapsNearby` | — |
| `DoMoveScan` | `RevealItems` | — |
| `DoMoveNoMove` | `TryInflictMuzzledStatus` | `FALSE` |

**All ten matched on the first candidate.**

## Notes on this batch

**Fact.** `DoMoveSeeTrap` and `DoMoveScan` forward to `RevealTrapsNearby` and
`RevealItems` — the first helpers in this family that are not
`TryInflict*Status`. They take the same two entity arguments, so the wrapper
shape is unchanged.

**Inference, weak.** None of the ten helpers is declared anywhere in the tree.
Nine take `(user, target)`, which the asm establishes. `TryInflictMuzzledStatus`
takes a third argument of `0`, named `onlyCheck` by the same positional analogy
used in the previous batches and equally unverified.

**Unverified in the same way as every batch here**: the `void` returns. These
wrappers discard whatever the helper returns, so nothing distinguishes `void`
from a value-returning helper.

## Tooling fixed rather than worked around

The previous batch hit a `build.sh` defect — it copies changed files into a
clean clone but had no way to express a *deletion*, and `extract_function.py`
removes an `asm/*.s` once emptied, which `common.mk`'s `$(wildcard asm/*.s)`
then picks back up. That batch was verified through a hand-written `docker run`.

`build.sh` now treats a listed path that is absent on the host as a deletion and
`rm`s it from the clone, so `git status --short` output can be passed wholesale.
This batch produced two deletions and verified through the normal path.

`build-tools/` is a workspace file, not part of the decompilation, so the fix is
an ordinary commit in the workspace repo and touches nothing upstream sees.
