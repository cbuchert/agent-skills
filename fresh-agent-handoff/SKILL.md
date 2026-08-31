---
name: fresh-agent-handoff
description: Write an operational handoff so a fresh agent can pick up long-running, multi-session work without losing measured state. Produces two artifacts — a handoff document committed to the repo, and a warmup prompt copied to the clipboard. Use at the end of a working session, when context is filling, or whenever the next turn of the work will be taken by an agent that was not present for this one.
argument-hint: "What the next session will pick up"
disable-model-invocation: true
---

# Fresh-agent handoff

Write the document a fresh agent needs to continue work it did not witness.

⚠️ **THIS IS NOT A CONVERSATION SUMMARY.** A summary compacts what was *said*. A handoff carries what
is *true* — measured counts, live infrastructure, decisions taken, traps discovered. **Most of that was
established by tool calls, not by discussion, and a compaction throws away exactly the part that
mattered.** If you find yourself narrating the session, stop and write down a number instead.

**Use it when:** a session is ending with work unfinished · context is filling · the next turn belongs to
an agent with no memory of this one · a human will pick it up after a gap.

**Do not use it for:** a finished, self-contained task (the diff and the commit message are the handoff) ·
a summary someone asked for to *read* rather than to *act on*.

---

## The two artifacts, and why they are different

| | **Handoff** | **Warmup** |
|---|---|---|
| Job | the **map** — everything true about the work | the **boot sequence** — what to do first |
| Length | as long as the truth takes | short enough to paste as a first message |
| Lives | **committed in the repo**, beside the code it describes | **on the clipboard**, and committed as a backup |
| Read | by the agent, in full, before acting | by the human, pasted to start the next session |

**Write both. The warmup is not a summary of the handoff — it is an instruction to read it, plus the
handful of things that are dangerous before you have.**

---

## Where they go

🚨 **COMMIT THEM TO THE REPOSITORY.** A handoff in a temp directory **cannot be diffed against the branch
it describes**, its citations cannot be checked against the commit that made them true, and it dies with
the machine. **Put it under a stable path** — `docs/engineering-handoff/`, `docs/handoff/`, wherever the
project keeps prose — **and commit it on the same branch as the work.**

⚠️ **Unless the user has said otherwise.** Some projects genuinely do not want this in git. **Ask once;
default to the repo.**

---

## The seven non-negotiables

These are what make a handoff survive contact with the next session. Everything else is arrangement.

### 1. 🚨 Re-measure every figure before you write it

**Do not copy a number from the previous handoff, from your own earlier message, or from a sub-agent's
report.** Run the command. Read the output.

⚠️ **AND WHERE YOU CANNOT VERIFY SOMETHING, SAY SO IN THE DOCUMENT** — *"this was claimed but I could not
reproduce it"* — rather than repeating it as fact. **An unverifiable claim marked as such is useful; the
same claim asserted is a landmine.**

**A count is only ever true of the commit it was measured at. Quote the commit with the count.**

### 2. 🚨 Lead with what needs the human

**Put it first, as a numbered list, before any state or history.** It is the bottleneck, and every other
section is idle until it clears. **For each item: what is blocked, what the decision is, and — where you
can — your recommendation, so the human can agree rather than compose.**

### 3. 🚨 Carry a self-doubt clause naming your predecessor's errors

**Open with a note on the document's own accuracy.** Say what the *previous* handoff got wrong — the
specific claim, not a general caution — and tell the reader to re-measure before believing this one.

**This compounds.** Once it is a convention, each handoff is checked by the next agent as a matter of
course, and the errors get named instead of inherited.

### 4. 🚨 Give the discovery command, never the value

**PIDs move. Session numbers move. Line numbers move — inside a single session.** Anything a reader could
re-derive, tell them how to re-derive it:

```
✅  lsof -nP -iTCP:5173 -sTCP:LISTEN          ❌  "the dev server is PID 62291"
✅  git rev-parse --short <branch>            ❌  "the branch is at abc1234"
✅  grep -n "functionName" path/to/file       ❌  "the guard is at :95"
```

⚠️ **Where you must give a value, give the command beside it and mark it as orientation.**
🚨 **Cite a SYMBOL as well as a line.** A merge landing between your grep and the reader's is enough to
invalidate every line number in the document.

### 5. 🚨 Never pin the document to an identifier it invalidates by existing

**Committing the handoff moves the branch past any SHA the handoff names.** So does committing the warmup.
**Do not name a tip SHA in either artifact** — tell the reader to read it.

**Pin a specific commit only where it is genuinely historical** — *"these counts were measured at X"* — and
say so explicitly.

### 6. 🚨 Traps, measured — not reasoned

**A trap section is only worth writing if every entry was actually hit.** For each: **what looked true,
what was true, and how it was measured.** No speculation, no "watch out for" without an incident behind it.

⚠️ **The highest-value trap is always one where a green result was wrong** — a test that cannot fail, a
command whose exit code is not what you think, a check that silently scanned nothing.
🚨 **AND DO NOT RELAY A TRAP YOU HAVE NOT TESTED.** Passing on someone else's warning as fact is how a true
statement becomes misleading advice. **If you did not verify it, mark it unverified.**

### 7. 🚨 Derived state is not duplication — restate it

**Reference issues, PRDs and ADRs by number or path rather than copying them.** But **derived** content —
counts, dependency graphs, file-contention sets, which tickets a new decision invalidated — **exists in no
other artifact and must be written down.** A reader should not have to open thirty issues to learn what is
dangerous.

**The test: could the reader reconstruct this by opening one document? Then link it. Could they only
reconstruct it by opening twenty and doing arithmetic? Then write it out.**

---

## Process

1. **Measure.** Re-run whatever the project's gate is, workspace by workspace, captured to files, and read
   exit codes **from the files**. Check live infrastructure by resource. Read the current state of the
   board. **Do this before writing a word.**
2. **Write the handoff** using [TEMPLATE.md](./TEMPLATE.md). Drop sections that do not apply; do not invent
   sections to fill.
3. **Write the warmup.** Front-load the things that are dangerous before the handoff has been read.
4. **Deliver the warmup to the clipboard** and **verify by round-trip** — `pbcopy < file`, then `diff
   <(pbpaste) file`. **Say the byte count.** A silently truncated warmup is worse than none.
5. **Commit and push both.** Then **re-read the tip** — you have just moved it.

---

## Calibration

**Length is set by how much is true, not by a target.** A one-week feature might need forty lines. A
multi-session orchestration with live infrastructure, a decision backlog and forty measured traps needs
several hundred, and compressing it loses the part that pays.

⚠️ **What you must not do is pad.** Every line should be a fact the next agent would otherwise have to
discover, a decision they must not re-litigate, or a trap they would otherwise fall into.

**If a section is empty, delete it. An empty "Traps" heading reads as "there are none", which is a claim.**
