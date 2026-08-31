# Handoff template

**Drop any section that does not apply. Do not invent one to fill.** An empty heading is a claim that
there is nothing there.

Sections 1–3 are the ones a reader acts on in the first five minutes. **Order matters: the human's
decisions come before the state, because the state is idle until they clear.**

---

```markdown
# <Project / epic> — session <N> handoff

**Written:** <date> · **Branch:** `<branch>` — read the tip, do not trust this document for it ·
**From:** the session-<N-1> orchestrator.

<One paragraph: what the reader is inheriting, in the present tense. Not a history — a state.
"You are inheriting a feature-complete, gated, unmerged epic; a corrected PRD already fanned out into
seventeen slice issues; three open defects with measured diagnoses; and live dev infrastructure.">

> 🚨 **A NOTE ON THIS DOCUMENT'S OWN ACCURACY.**
> <What the PREVIOUS handoff got wrong, specifically — the claim, not a general caution.>
> **Every figure below was re-measured on <date> before it was written**, and where something could not
> be verified **it says so rather than asserting it.** **Re-measure before you believe it.**

## 1. Read these first, in this order
<A table: path → why, one line each. Include the standing agent contract, the authoritative decisions
file, the running log, the spec. Mark anything SUPERSEDED in full, loudly.>

## 2. Your commission
<What the reader is for, and what they are not for. The working rules that shaped this session:
how to split work, what a green report is worth, what habit pays best.>

## 3. 🚨 WHAT NEEDS <HUMAN> — first, because it is the bottleneck
<Numbered. For each: what is blocked · the decision · your recommendation. Note which are one sentence
and which block whole lanes. THEN list what is NOT open any more, so nobody re-litigates it.>

## 4. Cost controls and irreversible actions
<CI minutes, PRs, deploys, anything billed or public. State the verified current value —
"the newest run is <date> and has not moved in three sessions" — so drift is visible.>

## 5. Live infrastructure
<Servers, ports, terminal sessions, containers. 🚨 GIVE THE DISCOVERY COMMAND, NOT THE VALUE.
Say what must stay running. Say what was deliberately removed and must not come back, with the reason.>

## 6. Working environment
<Worktrees/branches, setup recipe for a fresh one, config that is NOT at its default and why,
anything shared between agents that is not isolated by the usual boundaries.>

## 7. The merge / integration protocol
<How work lands. The mutex. What is blocked by tooling and the spelling that works. Who pushes.
What must never be left behind.>

## 8. State of play — the gate
<A table: check · result · exit code. 🚨 Quote the commit every count was measured at.
Derive deltas as equations. Name the counting rules that surprise — anything that moves a total
without a test being written.>

## 9. The board
<What landed. What is filed. What is blocked and by what. 🚨 File contention as its own graph —
it is invisible in the blocked-by field and it is what actually serialises work.>

## 10. The pattern that pays best
<How errors got caught. Name specific corrections that came UP the chain, including your own.
This is the section that makes the next agent argue with you, which is the point.>

## 11. Traps — all measured, none reasoned
<Every one an incident. What looked true · what was true · how it was measured.
Separate "learned this session" from "still binding from earlier".>

## 12. Briefing sub-agents
<The standing contract, plus what every brief must carry: refs, setup, prohibitions, the cleanup
contract, the files live peers hold, and "tell me which of my claims did not hold".>

## 13. If you do only one thing first
<An ordered start. Usually: verify the environment, re-run the gate yourself, put §3 in front of the
human, then dispatch.>
```

---

## Warmup template

**Separate artifact. Goes to the clipboard.** Its job is to be pasted as the first message of the next
session — so it must front-load anything dangerous **before** the handoff has been read.

```markdown
You are <role> for <project>, session <N>. <One line of history per prior session — what each one
established, not what it did.>

**Read `<path to handoff>` first, in full.** <Then the other required reading, in order, with what is
authoritative and what is superseded.>

**Your commission: <the one-line job>.**

🚨 **<THE MOST DANGEROUS THING, STATED AS A LIE THE ENVIRONMENT TELLS.>**
<e.g. wrong runtime version that invents failures; a config that makes a check silently pass;
a clock that is wrong in a way that reads as a product bug.>

🚨 **DO THIS ON YOUR FIRST TURN:** <the setup the human asked for by name, with the exact commands
and the verification that it worked BY RESOURCE, not by appearance.>

**You are inheriting live infrastructure. Do not rebuild it.** <Branch, worktrees, what is deliberately
gone, what is shared, what must not be reset.>

⚠️ **<Cost controls.>** <CI, PRs, anything billed. The verified current value.>

**Quality bar.** <Test discipline, the counting rules, the disguises of a false green found so far.>

🚨 **<THE HABIT THAT PAYS BEST.>** <Invite correction. Name your own errors from last session.>

**Start here:** <numbered, 4–6 items, ending in what to dispatch and in what order.>

**Report back** <what, and how often>. **Append close-out notes AS WORK LANDS.**
```

---

## Checks before you hand off

- [ ] Every count re-measured, and the commit quoted beside it
- [ ] Everything unverifiable **marked as unverifiable in the document**
- [ ] What needs the human is **first**, numbered, with recommendations
- [ ] The predecessor's specific errors named in the accuracy note
- [ ] **No tip SHA in either artifact** — discovery commands instead
- [ ] Every trap is an incident that actually happened
- [ ] Nothing relayed that you did not verify, unless marked
- [ ] Empty sections deleted, not left as headings
- [ ] Warmup on the clipboard, **verified by round-trip**, byte count stated
- [ ] Both artifacts committed and pushed; **tip re-read afterwards**
