# agent-plan-reviewer

A read-only adversarial plan-review subagent for Claude Code, Codex, and OpenCode. Hand it a plan; it tries to break the plan before you commit to it.

## What it does

Given a plan or design plus context, it distrusts every claim and verifies it against the real code (does the file, symbol, or API actually exist and behave as assumed?), then hunts for gaps, blind spots, edge cases, infeasibilities, intent divergence, and missed steps. It returns findings tagged `[BLOCKER]` / `[SHOULD-FIX]` / `[NIT]`, each with a concrete scenario, `file:line` evidence, and a suggested fix. It never edits anything; it can web-check an external API or library when unsure.

It carries only Read, Grep, Glob, Bash, WebFetch, and WebSearch (no MCP/connector tools). That least-privilege set is deliberate: an agent that advertises no connector schema cannot be taken down by a malformed one, so it stays spawnable when broader agents do not.

## When to use it

At a planning gate, before confirming a non-trivial plan (on Claude Code, before `ExitPlanMode`). Run two or three in parallel with disjoint lenses (correctness, edge cases, feasibility) and fold the findings back into the plan.

## Install

```bash
npx @ctxr/kit install @ctxr/agent-plan-reviewer
```

`@ctxr/kit` installs the bundle and mirrors it into the host's agent directory; the agent then appears as `agent-plan-reviewer`.

## Releasing

Releases are PR-gated and the publish step is a manual dispatch; the CI workflows (`ci`, `release`, `tag-on-main`, `publish`) carry the details.

## License

MIT
