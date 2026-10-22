# explain-actions

An AI skill that explains what an agent is about to do before it runs commands or uses tools.

- What the command or tool does and why it is needed.
- What it downloads, from where, and where it saves it.
- Whether downloaded code will be installed or executed.
- Which app opens, which files change, and what gets sent externally.
- Sensitive values redacted; more detail on request.

Install this folder as `explain-actions` in your agent's skill directory. For Codex, use `~/.agents/skills/explain-actions`; for Claude Code, use `~/.claude/skills/explain-actions`.

Invoke `$explain-actions` in Codex or `/explain-actions` in Claude Code. Explanations stay active for the conversation until disabled.

Instruction-based. It does not add buttons, intercept every tool call, or replace permission controls. No scripts, dependencies, or hooks.

Read [SKILL.md](SKILL.md) for the full behavior and source acknowledgments.
