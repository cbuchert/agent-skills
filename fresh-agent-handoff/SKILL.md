---
name: fresh-agent-handoff
description: Write an operational handoff and warmup prompt so a fresh agent can pick up long-running work.
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

## The commission the handoff must transmit

🚨 **THE READER'S CONTEXT WINDOW IS THE SCARCEST RESOURCE IN THE WHOLE OPERATION, AND THE HANDOFF MUST
SAY SO BEFORE IT SAYS ANYTHING ELSE.** An agent that spends its context writing code runs out partway
through and hands off badly, and **a bad handoff costs the next session more than the code was worth.**
**The chain is only as good as its weakest document.**

**So the handoff must establish, explicitly, that the reader's job is to orchestrate — not to implement.**

### Write these into the document. Do not assume they are obvious.

- 🚨 **ORCHESTRATE; DO NOT CODE.** **The reader's tokens go on briefing sub-agents and reviewing what they
  produce.** Implementation is delegated. ⚠️ **An orchestrator that "just quickly fixes" something has
  spent context on the one thing it could have bought for free.**
- 🚨 **PARALLELISE AS HARD AS THE WORK ALLOWS. More agents, smaller chunks.** A task that looks like one
  job is usually three that can run at once. **Bias toward dispatching too many rather than too few** —
  an idle agent costs nothing and a serialised queue costs the session.
- 🚨 **SPLIT ON FILE CONTENTION, NOT ON SUBJECT MATTER.** ⚠️ **This is the rule everyone gets wrong.**
  Two tickets on the same topic often touch different files and run fine together; two tickets on
  unrelated topics that both edit one file **cannot**, no matter what the dependency graph says.
  🚨 **"Blocked by: none" is true of DEPENDENCIES and false of PARALLELISM.** **Derive the contention set
  yourself, from the files each piece of work must touch, and write it into the handoff as its own
  graph** (§9). **It is invisible everywhere else.**
- 🚨 **A GREEN REPORT IS A CLAIM, NOT A FACT.** **Verify it yourself: the diff, the counts, and — most
  of all — the specific thing the agent says it did NOT touch.** ⚠️ **A RED REPORT IS ALSO A CLAIM.**
  Check the environment before you believe a failure; a wrong runtime or a stale artifact invents
  failures that look exactly like real ones.
- 🚨 **QUALITY IS THE ORCHESTRATOR'S JOB, AND IT IS DONE IN THE BRIEF.** A sub-agent will do what the
  brief says, precisely, including the wrong things. **Every brief carries: the standing contract, the
  ref, the setup recipe, the prohibitions, the files live peers are holding by name, an explicit cleanup
  contract to be answered item by item, and what "done" includes beyond "tests pass".**
- 🚨 **GROOM BEFORE YOU BUILD, AND USE A DIFFERENT AGENT FOR EACH.** **A filed ticket is a claim, not a
  fact.** A groomer verifies every assertion at the file and line, corrects the ticket in place, and
  **implements nothing** — one that also builds will rationalise its own ticket.
- 🚨 **DEMAND EVIDENCE, NOT ASSURANCE.** *"A test pins this"* is a claim to verify **at the assertion**.
  Require: **watch the red and say why it was the RIGHT failure · mutation-prove the harness · REVERT
  the mutation and state that you did.** ⚠️ **A mutation kill proves a test CAN fail; a red proves it was
  written BEFORE the behaviour existed. They are different claims.**
- 🚨 **BUILD BRIEFS THAT INVITE CORRECTION.** End every one with **"I expect to be corrected — tell me
  which of my claims did not hold."** ⚠️ **Downstream agents correcting the orchestrator is the
  highest-yield thing that happens in this pattern**, and it only happens if you ask for it in words.

⚠️ **AND SAY WHAT THE READER SHOULD DELEGATE EVEN WHEN IT FEELS FASTER TO DO IT.** Re-running the gate,
reading a long file, auditing a board, updating tickets — **all of it can be delegated, with the results
read back from files rather than from the agent's summary.** **Delegate the work; keep the judgement.**

---

## The five non-negotiables

These are what make a handoff survive contact with the next session. Everything else is arrangement.

### 1. 🚨 A handoff DECAYS. Write the derivation, not the value.

**Every fact in a handoff starts going stale the moment it is written, and some of it is stale before you
finish the sentence.** ⚠️ **The document cannot be a snapshot. It has to be a set of instructions for
getting a fresh one.** Three faces of the same rule:

**Re-measure before you write.** **Do not copy a number from the previous handoff, from your own earlier
message, or from a sub-agent's report.** Run the command; read the output. ⚠️ **And where you cannot
verify something, say so IN THE DOCUMENT** — *"this was claimed but I could not reproduce it"* — rather
than repeating it as fact. **An unverifiable claim marked as such is useful; the same claim asserted is a
landmine.** **A count is only true of the commit it was measured at, so quote the commit beside it.**

**Give the command, not the value.** **PIDs move. Session numbers move. Line numbers move inside a single
session.**

```
✅  lsof -nP -iTCP:5173 -sTCP:LISTEN          ❌  "the dev server is PID 62291"
✅  git rev-parse --short <branch>            ❌  "the branch is at abc1234"
✅  grep -n "functionName" path/to/file       ❌  "the guard is at :95"
```

⚠️ **Where you must give a value, put the command beside it and mark it as orientation.**
🚨 **Cite a SYMBOL as well as a line** — one merge landing between your grep and the reader's invalidates
every line number in the document.

**Never pin the document to an identifier it invalidates by existing.** 🚨 **Committing the handoff moves
the branch past any SHA the handoff names. So does committing the warmup.** **Name no tip SHA in either
artifact; tell the reader to read it.** **Pin a commit only where it is genuinely historical** — *"these
counts were measured at X"* — **and say so.**

### 2. 🚨 Lead with what needs the human

**Put it first, as a numbered list, before any state or history.** It is the bottleneck, and every other
section is idle until it clears. **For each item: what is blocked, what the decision is, and — where you
can — your recommendation, so the human can agree rather than compose.**

**Then list what is NOT open any more**, so the next session does not re-litigate a decision already taken.

### 3. 🚨 Name your predecessor's errors, and invite the same

**Open with a note on the document's own accuracy.** Say what the *previous* handoff got wrong — **the
specific claim, not a general caution** — and tell the reader to re-measure before believing this one.

**This compounds.** Once it is convention, each handoff is checked by the next agent as a matter of
course, and errors get named instead of inherited.

### 4. 🚨 Traps, measured — not reasoned

**A trap section is only worth writing if every entry was actually hit.** For each: **what looked true,
what was true, and how it was measured.** No speculation; no "watch out for" without an incident behind it.

⚠️ **The highest-value trap is always one where a green result was wrong** — a test that cannot fail, a
command whose exit code is not what you think, a check that silently scanned nothing.
🚨 **AND DO NOT RELAY A TRAP YOU HAVE NOT TESTED.** Passing on someone else's warning as fact is how a
true statement becomes misleading advice. **If you did not verify it, mark it unverified.**

### 5. 🚨 Derived state is not duplication — restate it

**Reference issues, PRDs and ADRs by number or path rather than copying them.** But **derived** content —
counts, dependency graphs, file-contention sets, which tickets a new decision invalidated — **exists in no
other artifact and must be written down.** A reader should not have to open thirty issues to learn what is
dangerous.

**The test:** could the reader reconstruct this by opening **one** document? Link it. Only by opening
twenty and doing arithmetic? **Write it out.**

---

## Process

**Each step ends on a criterion you can check.** ⚠️ **A vague one invites you to stop early on the step
that matters most — the measuring.**

1. **Measure.** Re-run the project's gate, workspace by workspace, captured to files, reading exit codes
   **from the files**. Verify live infrastructure by resource. Read the current board.
   **Done when: every figure that will appear in the document is traceable to a command you ran in this
   session — none inherited, none remembered.**
2. **Write the handoff** using [TEMPLATE.md](./TEMPLATE.md). Drop sections that do not apply; do not
   invent sections to fill.
   **Done when: every section is either populated or deleted, and every unverifiable claim is marked as
   unverifiable in the text.**
3. **Write the warmup.** Front-load what is dangerous **before** the handoff has been read.
   **Done when: it names no tip SHA, and a reader who acted on it alone would not break anything.**
4. **Deliver the warmup to the clipboard and verify by round-trip** — `pbcopy < file`, then
   `diff <(pbpaste) file`.
   **Done when: the diff is empty and you have stated the byte count.** ⚠️ **A silently truncated warmup
   is worse than none.**
5. **Commit and push both**, then **re-read the tip — you have just moved it.**
   **Done when: the working tree is clean, both artifacts are pushed, and nothing you wrote names the
   commit you just created.**

## Calibration

**Length is set by how much is true, not by a target.** A one-week feature might need forty lines. A
multi-session orchestration with live infrastructure, a decision backlog and forty measured traps needs
several hundred, and compressing it loses the part that pays.

⚠️ **What you must not do is pad.** Every line should be a fact the next agent would otherwise have to
discover, a decision they must not re-litigate, or a trap they would otherwise fall into.

**If a section is empty, delete it. An empty "Traps" heading reads as "there are none", which is a claim.**
