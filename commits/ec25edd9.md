# `ec25edd9` — Decomp five more leftover single-function asm files

| | |
|---|---|
| **Commit** | `ec25edd9` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `80d0e710` |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK` |
| **Note written** | with the commit |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The diff and the matching build are the sources of
> truth — see [the README](../README.md). Claims are labelled **fact** or
> **inference**.

## Files removed

**Fact.** Single-function asm files: **102 → 97** (112 before the sweeps).

| file removed | function |
|---|---|
| `asm/overlay_29_023021F0.s` | `UpdateIqSkillsWrapper` |
| `asm/overlay_29_022EBC98.s` | `SetActionUseMovePlayer` |
| `asm/overlay_29_02349658.s` | `ov29_02349658` |
| `asm/main_0205EBF0.s` | `sub_0205EBF0` |
| `asm/overlay_29_022F52B0.s` | `ov29_022F52B0` |

**Fact.** Two rest on types already in the tree rather than new placeholders:
`SetActionUseMovePlayer` uses `ACTION_USE_MOVE_PLAYER` (value 20, matching the
`mov r1, #0x14`) and `action_data::action_parameters[]`; `sub_0205EBF0` indexes
`mission_deliver_list::unk18`, the struct `src/main_0205C73C.c` already uses. A
placeholder was written for the latter first and then thrown away when the real
struct turned up — worth grepping `src/` and not just `include/` for a global's
type.

## A conflicting declaration that could NOT be retired

**Fact, and the most interesting thing here.** `src/overlay_29_022F0EDC.c`
(`SetLeaderAction`) carries a provisional `extern s32 SetActionUseMovePlayer();`
written from its call site. Now that the callee is decompiled, the process says
to delete that extern and include the real header — which declares it `void`.

**Doing so breaks the ROM.** `main.sbin`, `OVY_10` and `OVY_29` all fail their
checksums. `SetLeaderAction` only matches with the `s32` form.

So the two declarations still disagree, deliberately, and this commit does not
pretend otherwise. What that means is genuinely open:

- The asm ends `ldmia sp!, {r4, r5, r6, pc}` with nothing writing `r0` after the
  `bl SetMonsterActionFields`, so whatever that callee leaves in `r0` is what
  this function returns. A `void` reading is therefore not obviously right.
- But `SetMonsterActionFields` is itself declared `void` in `dungeon_action.h`,
  so "returns its callee's result" cannot be written in C directly.
- Either this function's return type is not `void`, or `SetLeaderAction`'s call
  site is written in a way that depends on a wrong prototype and is itself
  suspect.

**Not resolved here.** Recorded so it is not silently "cleaned up" by someone
deleting the extern and re-breaking the build. Contrast
[`25d5994b`](25d5994b.md), where retiring the equivalent `GetLeaderMonster`
extern worked and the ROM still matched.

## Weakest claim

**Inference.** `ov29_022F52B0` is a bare veneer (`ldr ip, =f` / `bx ip`) with no
argument setup, so nothing evidences its arity; `void(void)` is a guess, as with
the two empty functions in [`b088fd5f`](b088fd5f.md).

## Tried and abandoned

Two candidates were prepared and dropped rather than forced, both single-row
misses after several attempts:

- **`FixedPoint64ToInt`** (`asm/main_02001CB0.s`): the target loads both words
  with `ldm r0, {r1, r2}`; separate `value[0]`/`value[1]` reads and a
  struct-copy-to-local both fail to produce it (735 / 940).
- **`sub_0205B6CC`** (`asm/main_0205B6CC.s`): the target uses the
  `cmp` / `cmpeq` compound-condition idiom; `||`, a negated `&&`, and unsigned
  field types all emit branches instead (715 each).
