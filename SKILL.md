---
name: explain-actions
description: Explain proposed commands, tool calls, app launches, downloads, installations, and permission requests in plain language before execution. Use when the user wants to learn what an agent is doing or understand exactly what they are approving. Provide concise explanations with deeper detail on request.
---

# Explain actions

Explain the actual proposed action before running it. Once the user activates this mode, continue for the conversation until they disable it. Follow applicable host permissions; explanations do not grant approval or replace the real approval interface. Do not execute a command merely to explain it.

## Before an action

Give one short sentence saying what will happen and why it helps the user's request. Group repetitive low-impact reads/searches into a single explanation. Give downloads, installations, deletions, external writes, privilege changes, and app launches their own clear explanation.

Supply these details when relevant, putting consequential facts before execution rather than hiding them in optional detail:

- **Command or tool:** actual executable/tool name and arguments, target directory or document, and important flags. Describe pipes, redirects, command substitutions, and chained stages. For connectors, show the service, operation, and relevant parameters. Identify an indirect script as a script; inspect its relevant behavior before claiming to know what it does.
- **Downloads:** exact known package/file and requested version, source URL or package registry, destination, and purpose. State whether it is simply downloaded, installed, imported, or executed; mention installation hooks/scripts when known. Dependency managers can fetch transitive packages: distinguish the requested package from the full dependency set. Do not invent resolved versions, sizes, final redirect addresses, hashes, or complete lists. Use lockfiles/metadata when available and mark unknowns explicitly.
- **Apps and browser tools:** which app/service opens, which URL/file it receives, whether a visible window or background process is expected, and what action the browser will take. For searches, give the search intent; avoid claiming a search result is a download.
- **Effects:** relevant files read/changed/deleted, external recipient of uploads or requests, possible costs, persistence, and whether elevated access is requested. Explain practical consequences, not a blanket risk rating based solely on tool type.
- **Recovery:** meaningful undo route, if available. Never label permanent deletion or an external message reversible.

Never display credentials, auth headers, signed URL query tokens, passwords, private environment values, or sensitive file contents to explain an action. Redact those portions and label the displayed command as redacted. Do not silently alter execution parameters to match an explanation.

## Progressive detail

Default to a short explanation plus a compact command/tool preview when useful. Offer a deeper flag-by-flag explanation on request. If the host demonstrably renders collapsible Markdown, optional background can use `<details><summary>Command details</summary>...</details>`; otherwise use a short plain-text block. Never promise a new button or modify the host's command/approval cards. The skill cannot intercept all actions or inspect hidden tool internals.

Example:

> I'll download the project dependencies so its tests can run. `npm ci` uses the versions in package-lock.json and replaces node_modules in this project. It can also execute dependency installation scripts. I'll check the configured registry before naming the download source.
>
> Command: `npm ci` — working directory: the authorized project folder.

For an approval request, put the concrete action and consequence in the host's justification field where supported. Explain what declining prevents only when asking for a decision. Do not create new permission prompts for already authorized routine work.

After execution, summarize the result and any material difference from the preview. If the action changes materially (different source, target, permissions, or execution behavior), explain the revised action before retrying. If permission review blocks an action, report the stated reason; do not describe it as the user declining.

## Hosts and provenance

Codex: invoke as `$explain-actions`. Claude Code: use the same skill instructions in its skill directory. This is an instruction-based mode, not a guaranteed interception mechanism. Claude Code PreToolUse hooks are a separate integration; this package does not install hooks or alter host settings. See [Claude hooks](https://code.claude.com/docs/en/hooks).

Conceptually informed by [Noob Mode in github/awesome-copilot](https://github.com/github/awesome-copilot/blob/main/skills/noob-mode/SKILL.md) and [Anthropic's explanatory output plugin](https://github.com/anthropics/claude-code/blob/main/plugins/explanatory-output-style/README.md). Independently written for command transparency, concrete consequences, redaction, and progressive detail; no upstream text or scripts are bundled.
