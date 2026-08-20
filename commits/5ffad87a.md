# `5ffad87a` — Decomp five leftover single-function asm files

| | |
|---|---|
| **Commit** | `5ffad87a` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `159235da` |
| **Verified** | matching build, `build/pmdsky.us/pmdsky.us.nds: OK` |
| **Note written** | with the commit |

> **Unverified AI-authored reasoning.** Not part of the decompilation, never
> merged, not authoritative. The diff and the matching build are the sources of
> truth — see [the README](../README.md). Claims are labelled **fact** or
> **inference**.

## Files removed

**Fact.** Single-function asm files: **63 → 58** (112 before the sweeps).

| file removed | function |
|---|---|
| `asm/main_0205B6CC.s` | `sub_0205B6CC` |
| `asm/main_0202C75C.s` | `sub_0202C75C` |
| `asm/overlay_29_0232CDA4.s` | `DoMoveOneShot` |
| `asm/overlay_29_0232A078.s` | `DoMoveNightShade` |
| `asm/overlay_29_023016D8.s` | `DisplayRunAwayIfTriggered` |

**Fact.** All five merged into an adjacent existing source; none split a `.s`.
**Fact.** None carries a region conditional, so US alone verifies this commit.

## A stand-in context produced a false 0

**This is the important entry, and it is a near-repeat of the mistake on
[`19c62d4b`](19c62d4b.md).**

**Fact.** `sub_0202C75C` was first scored against a *hand-written* stand-in
struct — a plausible layout invented to get the offsets right, rather than the
tree's real one. It scored **0**.

**Fact.** Re-scored against the real `struct unk_0202C5E0` from
`include/main_0202C5E0.h`, the *same* candidate scored **500**. The stand-in had
made `w + 4` a named struct member, which changed nothing, but it had also let
the two early exits compile as separate blocks. The target shares one `-1` exit,
which only the real layout exposed:

```c
/* 500 -- two independent early returns */
if (w->field_0x1B0 == 0) { return -1; }
if (w->field_0x1B1 != 0) { return -1; }
return GetSelectedMenuItemIdx((void *) w + 4);

/* 0 -- one shared exit at the end */
if (w->field_0x1B0 != 0) {
    if (w->field_0x1B1 != 0) {
        return -1;
    }

    return GetSelectedMenuItemIdx((void *) w + 4);
}

return -1;
```

**The rule this argues for:** a stand-in context is fine for *finding* the shape,
but the score is only evidence once the context is the tree's own headers. Two
sweeps in a row have now had a 0 evaporate when the real types went in.

**Fact.** The struct was found rather than invented. The function immediately
before it in `main.lsf`, `GetWindowIdSelectedMenuItemIdx`, is already decompiled
as `GetSelectedMenuItemIdx(GetWindowContents(window_id) + 4)` — the same `+ 4`,
on `void *`, which is where the spelling comes from. `struct unk_0202C5E0`'s
`u8 field_0x1AC[6]` covered `0x1B0`/`0x1B1` and is split into three members here;
no new struct was introduced.

## Two more things the scratch could not see

**Fact.** After the stand-in problem above was fixed and all five scored 0, the
real build still failed **twice**, for reasons no scratch can surface:

1. **A pre-existing declaration.** `include/main_02042AF8.h` already carried
   `s32 sub_0202C75C(s8 a);`, written from the single call site in
   `src/main_02043380.c`, which passes an `s8` field. The parameter is a window
   id and is `s32`. That file already included the real header, so the conflict
   fired the moment the prototype landed. **I did not run the stale-declaration
   grep before landing**, which the project's own procedure requires; the build
   caught it instead. This is the fourth sweep in a row to turn up a provisional
   declaration that was narrower than the real signature.
2. **Missing includes.** `GetMoveType` and `GetMoveCategory` were in the scratch
   context but not `#include`d by either `DoMove` destination file, giving
   `function has no prototype`. The scratch context is generated from the whole
   header set, so it hides which headers a file actually needs.

## `FixedPoint64ToInt` — not landed, stands at 225

**Fact.** The target keeps a mask that is arithmetically redundant:

```asm
	mov r0, #0x10000
	rsb r0, r0, #0            @ 0xFFFF0000
	lsl r1, r1, #0x10
	and r0, r2, r0
	orr r0, r1, r0, lsr #0x10
```

`(lower & 0xFFFF0000) >> 16` is just `lower >> 16`, and MWCC folds it — the
candidate emits a bare `lsr` and scores **735**.

**Fact, and the one real finding.** Whether the mask survives is decided by
**which side of the `|` the masked term is written on**:

| spelling | score |
|---|---|
| `(upper << 16) \| ((lower & -0x10000) >> 16)` | 735 — mask folded away |
| `((lower & -0x10000) >> 16) \| (upper << 16)` | **225** — mask kept |

Ten spellings were tried and every one that keeps the mask lands on exactly
**225**, and every one that does not lands on 730/735. The plateau is hard.

**Fact.** What remains at 225 is one register and the order of two instructions.
The target computes `upper << 16` *before* the `and` and lets the `and` overwrite
the mask constant's register (`and r0, r2, r0`); the candidate computes the `and`
first into a fresh register (`and r2, r3, r0`), which costs it a third register.

```
T  0: ldm r0, {r1, r2}      C  0: ldm r0, {r1, r3}
T  c: lsl r1, r1, #0x10     C  c: and r2, r3, r0
T 10: and r0, r2, r0        C 10: lsl r0, r1, #0x10
T 14: orr r0, r1, r0, lsr #0x10   C 14: orr r0, r0, r2, lsr #0x10
```

**The bind, stated plainly.** Writing the masked term first is what preserves the
mask, and writing it first is also what makes it evaluate first. The target needs
the mask preserved *and* evaluated second. No spelling found so far does both.

**Falsified** (all 730 or worse — the mask folds): masked term second in any
form, a named local for the masked value used second, `+` instead of `|`,
`u32` result, `~0xFFFF` instead of `-0x10000`, `* 0x10000` / `/ 0x10000` instead
of shifts, and a local struct copy (920). **Falsified** (1020): spelling the
whole thing as a real 64-bit shift,
`(s32)((((s64) upper << 32) | lower) >> 16)`, in `s64` and `u64` forms — this is
the most natural reading of the function's name and it is not what the compiler
did.

**Untried directions**, for whoever picks it up: making the mask constant reach
the expression from somewhere MWCC cannot fold through (a `const` local, a
function argument); changing `struct fixed_point_64`'s member types; and reading
`FixedPoint64ToInt`'s callers in `asm/overlay_29_0230BBAC.s`, which have not been
looked at and may show the result being used in a way that constrains the
expression.

## `sub_0205B6CC` — a 64-bit compare, and `u64` at a 4-aligned offset

**Fact.** The body is a 64-bit unsigned comparison against 2:

```asm
	ldr r1, [r0, #0x10]
	ldr r0, [r0, #0xc]
	cmp r1, #0
	cmpeq r0, #2
	movhs r0, #1
```

`cmp`/`cmpeq` with `hs` is the standard 64-bit `>=`; the high half is at `0x10`
and the low at `0xC`.

**Fact.** `>= 2` scores 0 and `> 1` scores 310 — the same predicate, different
code.

**Fact, worth knowing.** A `u64` member at offset `0xC` works: MWCC aligns
`long long` to **4** here, not 8, so `u8 field_0x0[0xC]; u64 field_0xC;` places
the member where the asm reads it. Reconstructing the value from two `u32`s
instead scores 200.

**Inference.** `struct unk_0205B6CC` is a placeholder named for the function, per
the naming rules — the sole caller passes an interior pointer (`r1 + 0x20` off
another object), so there is no global to name it after and no way to tell what
the surrounding object is.

## `DoMoveOneShot` / `DoMoveNightShade` — an `enum` temporary costs a mask

**Fact.** These two are near-identical: both stage eleven arguments for
`CalcDamageFixedWrapper`, differing only in the fixed damage (`0x270F` versus the
attacker's level) and in `DoMoveNightShade` reading that level first.

**Fact.** The only obstacle was the type of the temporary holding
`GetMoveType`'s result. Declared as `enum type_id` it scores **950**, because
`-enum min` sizes that enum to one byte and the assignment emits
`and r5, r0, #0xff` that the target does not have. As `s32` it scores **0**;
`u32` also scores 0.

**Inference.** Temporaries are needed at all because the three helper results are
computed in source order (`GetMoveType`, `GetMoveCategory`,
`GetDamageSourceWrapper`) while MWCC evaluates call arguments right to left.

**Fact.** `CalcDamageFixedWrapper` and `GetDamageSourceWrapper` had **no**
existing declaration anywhere in `src/` or `include/` — grepped before landing.
The eleven-parameter prototype is read from the call site and is entirely a
guess about types; only the count and the register/stack positions are facts.

## `DisplayRunAwayIfTriggered` — an existing NONMATCHING guess was right

**Fact.** The HP test is
`info->hp <= Min(999, info->max_hp_stat + info->max_hp_boost) / 2`.

**Fact.** `Min(999, max_hp)` scores 0 and `Min(max_hp, 999)` scores 200 — the
argument order is not free, because `Min` is a `static inline` that expands to a
comparison.

**Fact, and a pleasing one.** That exact expression, in that exact argument
order, already existed in the tree — inside the `#ifdef NONMATCHING` block of
`AiMovement` in `src/dungeon_ai_movement.c`, written by someone else for a
different function that still does not match. The guess was right; it was just
never confirmed anywhere.

**Fact.** `UpdateStateFlags` is called unconditionally and its result is only
then combined with the parameter:

```c
result = UpdateStateFlags(info, 4, low);
if (show_effect && result) {
    PlayEffectAnimation0x29(entity);
}
```

The target's `cmp r5, #0` / `cmpne r0, #0` tests the parameter first, after the
call has already happened.

## Naming

**Fact.** No function renamed. One new type, `struct unk_0205B6CC`, named for the
function that takes it, with `field_0x<off>` members. `struct unk_0202C5E0` was
already in the tree and only had one member split.

## Open questions for a reviewer

1. **`CalcDamageFixedWrapper`'s eleven parameters.** Types are guesses; several
   are literal `0`/`1` whose meaning is unknown. `s32 a` (the fourth, always `1`)
   and the trailing `0, 1, 0` are especially opaque.
2. **`UpdateStateFlags(info, 4, low)`.** The `4` is a bare literal; no enum or
   named constant was found for it.
3. **`struct unk_0205B6CC`.** Only offsets `0xC`–`0x13` are used. The caller
   passes an interior pointer, so the type may well be a *member* of a larger
   struct that already exists under another name.
4. **`sub_0202C75C`'s return of `-1`** on both failure paths — whether the two
   conditions mean different things is not established.
5. **`DisplayRunAwayIfTriggered`'s `999` cap** is a bare literal here, matching
   the existing `AiMovement` code; a named constant may be warranted.
