# `bee91dbc` — Decomp five team-member index accessors

| | |
|---|---|
| **Commit** | `bee91dbc` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-team-member-accessors`, on top of `cb366895` |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK` |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The PR diff and the matching build are the sources
> of truth — see [the README](../README.md). Claims are labelled **fact** (read
> off the asm, or from an in-tree header) or **inference**.

The index-returning counterparts of the pointer accessors in `cb366895`.

## What they do

**Fact.** Each returns the index of a `TEAM_MEMBER_TABLE_PTR->members` entry when
that entry's `is_valid` bit is set, else -1:

| function | index | else |
|---|---|---|
| `GetHeroMemberIdx` | 0 | -1 |
| `GetPartnerMemberIdx` | 1 | -1 |
| `GetMainCharacter1MemberIdx` | 2 (special episode) / 0 | -1 |
| `GetMainCharacter2MemberIdx` | 3 / 1 | -1 |
| `GetMainCharacter3MemberIdx` | 4 (special episode only) | -1 |

No new names — all types and the five labels pre-exist.

## The return type is the whole story

These matched only once the return type was corrected from `int` to **`u32`**,
and the reason is worth recording because it cost a long search.

- **Fact.** With a signed `int` return, the compiler recognises `valid ? 0 : -1`
  as the idiom `-(!valid)` and emits an arithmetic negate. The target uses a
  conditional-move **select**. Under `u32`, `-1` is `0xFFFFFFFF`, the negate
  idiom does not apply, and the compiler emits the select.
- **Fact.** The distinction is visible **only** for `GetHeroMemberIdx`, whose
  index is the constant `0` — the value that makes `valid ? 0 : -1` fusable. The
  four siblings have a non-zero or runtime index, so `valid ? idx : -1` uses the
  select under *either* signedness, and they matched even as `int`. The family
  is typed `u32` uniformly because the one function that distinguishes the two
  types requires it.
- **Fact.** `sub_02065050` (commit `d34766ea`) calls `GetMainCharacter1MemberIdx`
  and `GetMainCharacter2MemberIdx`. It was rebuilt with the `u32` prototypes and
  is byte-identical — the equality comparisons it does on the result don't
  depend on signedness.

**The lesson**, generalised into `docs/MATCHING_TIPS.md`: the function
*signature* — return type and parameter types — is part of the search space, not
a fixed frame. This is the third signature-driven match on this cluster
(`sub_02055410`'s parameter, `GetScriptEntityMonsterId`'s frame, and now this
return type). Roughly 45 body rewrites failed here because the signature was held
fixed; a community member matched it immediately by changing the return type.

## Open

- `GetAppointedLeaderMemberIdx` and `sub_02056914` remain in the cluster;
  `sub_02056914` is larger (reads the roster region, needs more struct mapping).
