# agent-skills

Skills for Claude Code, kept in git so they can be versioned, reviewed and shared.

## Install

Skills are installed in **two hops**, so the repo stays the source of truth and one store serves every
agent tool:

```sh
# 1. the tool-agnostic store points at this repo (absolute)
ln -s "$PWD/fresh-agent-handoff" ~/.agents/skills/fresh-agent-handoff

# 2. Claude Code points at the store (relative, matching its siblings)
cd ~/.claude/skills && ln -s ../../.agents/skills/fresh-agent-handoff fresh-agent-handoff
```

**Why not link `~/.claude/skills` straight at the repo?** Because `~/.agents/skills/` is the canonical
store — every other skill is reached through it, and other agent tools point at the same directory. A
skill wired directly into `~/.claude/skills` works, but it is the only one shaped differently, and that
is what gets "tidied" later by someone who does not know why.

**Verify the chain resolves to the repo:**

```sh
readlink -f ~/.claude/skills/fresh-agent-handoff   # → …/agent-skills/fresh-agent-handoff
```

Editing here updates the installed skill with no re-copy. **No restart is needed** — a newly linked skill
is picked up by the running session.

⚠️ These skills set `disable-model-invocation: true`, so they are **user-invoked only**: type
`/fresh-agent-handoff`. An agent cannot fire one on its own, which is deliberate — writing a handoff is
expensive and explicit.

## Skills

| Skill | What it does |
|---|---|
| [`fresh-agent-handoff`](./fresh-agent-handoff) | Writes an operational handoff so a fresh agent can pick up long-running, multi-session work without losing measured state. Produces a handoff document committed to the repo and a warmup prompt on the clipboard. |

### `fresh-agent-handoff`

Written after six sessions of a multi-agent orchestration where each session inherited the previous
one's work through a handoff document, and where **each handoff was found to contain a specific wrong
claim by the session that inherited it** — a moved commit, a dead terminal session, a blocker that had
been fixed before it was read.

The skill encodes what stopped that happening: re-measure every figure before writing it, say what you
could not verify, lead with what needs the human, give discovery commands rather than values, and never
pin the document to an identifier it invalidates by existing.

It also encodes the operating model that made the chain work: **the incoming agent orchestrates and
does not code.** Its context is the scarce resource, so it spends it briefing sub-agents and reviewing
what they produce — parallelising hard, **splitting on file contention rather than subject matter**, and
treating every green report as a claim to be verified rather than a fact.

⚠️ It is deliberately **not** a conversation summariser. A summary compacts what was said; a handoff
carries what is true, and most of that was established by tool calls rather than discussion.
