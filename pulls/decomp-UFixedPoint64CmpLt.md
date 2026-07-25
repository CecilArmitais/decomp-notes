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
| `14c23438` | Decomp UFixedPoint64CmpLt | [notes](../commits/14c23438.md) |

## What it does

`UFixedPoint64CmpLt` compares two 64-bit values held as high/low word pairs and
returns whether the first is less than the second.

`asm/main_02001A30.s` contained only this function, so the file and its `.inc`
are **removed** rather than split, the function merges into the adjacent
`src/main_020018D0.c`, and `main.lsf` drops `asm/main_02001A30.o`. Six files,
+13/−23.

## For reviewers

- **The signature is read off the target, not chosen.** Four separate parameters
  rather than two structs by value; unsigned rather than signed; a word-sized
  return rather than a byte. Each was tested alone with the other two held
  fixed — the scores, and the exact instruction that appears in each failing
  case, are tabulated in the [commit note](../commits/14c23438.md).
- **`BOOL` rather than `int` is the one judgement call here**, and it is free to
  overrule. `BOOL` is `typedef int` in the Nitro headers, so it is
  codegen-identical to the `int` this branch originally used; it was chosen to
  name the function's boolean semantics. `bool8` was suggested and does **not**
  match — it is `typedef u8`, and the truncation it forces
  (`and r0, r0, #0xff`) is absent from the target.
- **The commit also removes a stale prototype.** `src/main_02001BB4.c` declared
  this function itself, from its call site, before it was decompiled; that line
  is replaced by an include of the header. Its parameter types were signed,
  which the target contradicts. The two declarations never appeared in the same
  translation unit, so no build could have caught the disagreement. This means
  the caller's object genuinely changes — verified byte-neutral: both objects
  recompile and the ROM still matches.
- **No new names.** `UFixedPoint64CmpLt` was already labelled; no structs,
  fields or symbols are introduced.
- **This function had a decomp.me scratch link in the asm**, meaning someone had
  attempted it before. Worth knowing that our own earlier attempt also failed,
  and the per-commit note records why honestly: the failure was a wrong
  compiler id in our tooling, not a property of the function. Once corrected,
  the plainest possible C matched immediately.

## Open questions

- **`BOOL` vs plain `int` is a convention question the bytes cannot settle.**
  Both score 0. The tree currently uses `bool8` 193 times against `BOOL` 3
  times, so `BOOL` is rare here — but `bool8` is unavailable for this function,
  and a word-sized boolean has to be spelled somehow. If the project would
  rather see plain `int`, that is a one-word change with no effect on the
  output.
