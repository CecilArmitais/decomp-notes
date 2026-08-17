# `d4b2c2fc` — Decomp two bag helpers that forward to asm callees

| | |
|---|---|
| **Commit** | `d4b2c2fc` (as of writing — renamed if amended or rebased) |
| **Branch** | `decomp-continued`, on top of `27c9ec9a` |
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
| `RemoveFirstUnequippedItemOfType` | `0x0200F798` |
| `AddItemToBagNoHeld` | `0x0200F874` |

**Fact.** Both are thin forwarders. `AddItemToBagNoHeld` tail-calls
`AddItemToBag` with a zero second argument, which the target reaches with `bx`
rather than `bl`. `RemoveFirstUnequippedItemOfType` feeds
`GetFirstUnequippedItemOfType`'s result straight into `RemoveItemNoHoleCheck`.

## The return types are a guess, and are labelled as one

**This is the part of the commit worth a reviewer's attention.**

**Fact.** Neither function's bytes determine its return type. Both forward
whatever the callee returns **without touching it**, so declaring them `void`
scores 0 as well.

**Inference, explicitly weak.** They are written as returning `u32` because that
is what a pass-through of a word-sized result reads as. **A reviewer should treat
the return type as a guess rather than something the target settles.**

This is the same class of underdetermination as
[`e6c5c875`](e6c5c875.md)'s parameter count and [`b6faa493`](b6faa493.md)'s
parameter width -- and here, unlike those, there is no decompiled caller to
settle it.

## Provisional prototypes

**Fact.** All three callees are still asm and had **no declaration anywhere in
the tree** (checked). Their prototypes are declared in the new headers with the
loosest types that compile, and should collapse into the callees' own headers
when those land.

## Naming

Nothing new.
