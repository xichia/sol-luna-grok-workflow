# Project workflow

For a new piece of work or a request to resume a run, use
`.agents/skills/sol-luna-grok/SKILL.md`.

The manager is GPT-6.1 Sol. Explicitly delegate substantive implementation and
independent local review to GPT-6 Luna subagents; use Luna for bounded research
where it helps. Complete a staged plan
and obtain the user's approval before product implementation. Require manual
external Grok 4.7 review after every implementation stage, including review of
material repairs. Sol owns assignments, review prompts, decisions, run logs,
and the final report. Keep runs in `runs/<unique-run-id>/`.

A delegated subagent executes only its packet; it must not recursively run the
manager workflow, spawn children, or change manager-owned records.

Ordinary questions about the workflow and edits to this workflow package do
not themselves start a new staged run. Apply the full workflow to maintenance
only when the user asks to run that maintenance through it.
