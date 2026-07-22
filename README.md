# decomp-notes

Reasoning notes for AI-authored contributions to
[pret/pmd-sky](https://github.com/pret/pmd-sky), written by Claude Code and
linked from the pull requests they accompany.

## Read this before trusting anything here

**Everything in this repository is unverified AI-authored prose. It is not part
of the decompilation, it is never merged, and it is not authoritative.**

The sources of truth are, in order:

1. the **PR diff** — the code actually being proposed;
2. the **matching build** (`build/pmdsky.us/pmdsky.us.nds: OK`) — the only thing
   that proves a decompilation is correct;
3. **pmdsky-debug** — for names, types and layouts.

These notes exist because a byte-for-byte match proves what the code *does*, and
proves nothing about what it *means*. Every statement here about meaning —
what a field is for, what a function does, what an unnamed symbol should be
called — is inference, and some of it will be wrong. Claude has produced
confidently-worded and incorrect analysis in this project before; that is
precisely why this reasoning lives out here where it can be checked and
discarded, rather than as comments or invented names inside the decompilation.

So: treat a note as *"here is the reasoning to check"*, never *"here is the
answer"*. If a claim in a note matters, verify it against the asm.

## Why a separate repository

The decompilation forbids Claude-authored comments (see pmd-sky `CLAUDE.md` →
*Never add comments to pmd-sky*), because an inaccurate comment is unfalsifiable
by the build and misleads later work. Notes here get the reviewer-facing context
without putting any of it in the tree:

- a separate repo **cannot** appear in a pmd-sky pull request;
- `pmd-sky/` stays parked on its working branch — no branch swapping;
- notes are pushed in parallel with the branch and linked from the PR body.

## Layout

```
commits/<short-hash>.md    one note per pmd-sky commit
```

**Notes are keyed by commit**, so a PR that contains three commits links three
notes, and a reviewer reading any one commit can go straight to the reasoning
behind it. Each note opens with a header giving the commit, its branch, and how
it was verified.

Hashes move: these branches get amended, rebased and squashed routinely, so a
note is **renamed to the new hash** whenever its commit changes. The hash in the
header is always "as of writing".

One consequence worth knowing: a commit message can't contain a link to its own
note, because writing the message changes the hash. Notes are therefore linked
from the **PR body**, not from commit messages.

Each note covers, for the change it documents:

- **what was decompiled or identified**, and the evidence for each conclusion;
- **naming decisions** — what was named, what was deliberately left as a
  `sub_<addr>` / `unk_<ADDR>` / `field_0x<off>` placeholder, and why;
- **assumptions and open questions** — what is inferred rather than proven, and
  what a reviewer should push back on;
- **suggested names** for still-unidentified symbols, flagged as suggestions;
- **what was tried and failed**, for near-matches.

A note records **where the work stands**, never a verdict on the code. In
particular, nothing here is "unmatchable": retail was compiled from ordinary C
by the same toolchain, so a matching source exists for every function by
construction. "I did not find it, here is what I tried" is the only honest way
to write up a near-match.
