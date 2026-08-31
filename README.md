# agent-skills

Skills for Claude Code, kept in git so they can be versioned, reviewed and shared.

## Install

Skills live in `~/.claude/skills/<name>/`. Each top-level directory here mirrors that layout, so
installing is a copy or a symlink:

```sh
ln -s "$PWD/fresh-agent-handoff" ~/.claude/skills/fresh-agent-handoff
```

Symlinking keeps the installed skill in step with the repo. Copy instead if you want them to drift
independently.

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
