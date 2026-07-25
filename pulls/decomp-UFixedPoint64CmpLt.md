# `decomp-UFixedPoint64CmpLt` — Decomp UFixedPoint64CmpLt

| | |
|---|---|
| **Branch** | `decomp-UFixedPoint64CmpLt` |
| **PR** | *(fill in once opened)* |
| **Base** | `upstream/main` @ `bee717cd` |
| **Commits** | 1 |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK` |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The PR diff and the matching build are the sources
> of truth — see [the README](../README.md).

One function, no shared-type changes, nothing carried over from another branch.

## Commits

| commit | what | note |
|---|---|---|
| `f18c7dd5` | Decomp UFixedPoint64CmpLt | [notes](../commits/f18c7dd5.md) |

## What it does

`UFixedPoint64CmpLt` compares two 64-bit values held as high/low word pairs and
returns whether the first is less than the second.

`asm/main_02001A30.s` contained only this function, so the file and its `.inc`
are **removed** rather than split, the function merges into the adjacent
`src/main_020018D0.c`, and `main.lsf` drops `asm/main_02001A30.o`. Five files,
+12/−21.

## For reviewers

- **The signature is read off the target, not chosen.** Four separate `u32`
  parameters rather than two structs by value — a 2-word struct argument makes
  the compiler spill and reload all four argument registers, which the target
  does not do. The return is `int` rather than a byte type — a byte return
  appends `and r0, r0, #0xff`, which the target does not have.
- **No new names.** `UFixedPoint64CmpLt` was already labelled; no structs,
  fields or symbols are introduced.
- **This function had a decomp.me scratch link in the asm**, meaning someone had
  attempted it before. Worth knowing that our own earlier attempt also failed,
  and the per-commit note records why honestly: the failure was a wrong
  compiler id in our tooling, not a property of the function. Once corrected,
  the plainest possible C matched immediately.

## Open questions

None. The function is small, fully matched, and touches no shared types.
