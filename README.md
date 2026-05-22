# dispatching-parallel-agents (Claude Code skill)

A rewritten Claude Code skill for dispatching multiple subagents in parallel using the current `Agent` tool. Modernizes the dispatch guidance with the live `Agent` tool's parameter surface (`subagent_type`, `isolation`, `run_in_background`, `model`, `name` + `SendMessage`), corrects the obsolete `Task(...)` tool name, and adds a "read the diff, not the summary" verification pattern.

MIT licensed. Drop it into any Claude Code installation.

## Why this exists

The original `dispatching-parallel-agents` skill in the `superpowers` plugin still taught the legacy `Task(...)` tool name, never stated the rule that parallelism requires multiple tool calls in a SINGLE assistant message, and didn't cover modern `Agent` options like `isolation: "worktree"` and `run_in_background`. A baseline probe showed even fresh subagents reaching for `TaskCreate` and inventing a separate "EnterWorktree" step. This rewrite was developed with a RED/GREEN methodology (baseline failure → write skill → verify with a fresh subagent) and submitted upstream as [obra/superpowers#1611](https://github.com/obra/superpowers/pull/1611). This repo is the MIT-licensed standalone home for the rewrite, independent of the upstream merge fate.

## What it teaches

1. **The hard rule:** parallel dispatch requires ONE assistant message containing MULTIPLE `Agent` tool calls. Two consecutive messages with one Agent each run sequentially.
2. **`subagent_type` selection table** for jobs ranging from grep/locate work (`Explore`) to architecture design (`Plan`, `feature-dev:code-architect`) to independent code review.
3. **Useful Agent options:** `isolation: "worktree"` as default-on for parallel file edits, `run_in_background` semantics, `model` override, and `name` for `SendMessage` continuation.
4. **"Never delegate understanding":** synthesis stays with the dispatcher.
5. **Verification by diff, not summary.** An agent's summary describes what it intended to do; the diff is what it did.

## Install

### Claude Code (personal scope)

```bash
mkdir -p ~/.claude/skills/dispatching-parallel-agents
curl -fsSL https://raw.githubusercontent.com/dotcomjack/parallel-agents-skill/main/SKILL.md \
  -o ~/.claude/skills/dispatching-parallel-agents/SKILL.md
```

Next session, the skill appears in the available-skills list and can be invoked when you face 2+ independent problems.

### As a plugin override

If you're using the `superpowers` plugin and want to override its bundled version locally until the upstream PR lands:

```bash
cp SKILL.md ~/.claude/plugins/cache/claude-plugins-official/superpowers/5.1.0/skills/dispatching-parallel-agents/SKILL.md
```

Note: plugin updates may overwrite this. Re-apply after `claude` plugin updates, or wait for upstream merge.

## License

MIT. See [LICENSE](./LICENSE). The skill content is derivative of [obra/superpowers](https://github.com/obra/superpowers), also MIT-licensed.

## Upstream PR

[obra/superpowers#1611](https://github.com/obra/superpowers/pull/1611). If it merges, this repo becomes a historical mirror. If not, this remains the canonical MIT-licensed home for the rewrite.
