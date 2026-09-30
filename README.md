# Sol · Luna · Grok workflow

A reusable Codex project workflow: GPT-6.1 Sol manages, GPT-6 Luna subagents do
bounded work, and you carry review packets to external Grok 4.7.

## Start a piece of work

Open this project in Codex and select **GPT-6.1 Sol** in the model picker. Start a
new chat if the newly added skill and configuration are not yet discovered.
Then send:

```text
Use $sol-luna-grok for the following work: [describe the outcome and constraints].
First prepare the complete staged plan and wait for my approval.
After approval, delegate implementation to GPT-6 Luna and require external
Grok 4.7 review at the end of every implementation stage.
```

If the skill is not listed, ask Codex to read
`.agents/skills/sol-luna-grok/SKILL.md` and follow it. The same instructions are
linked from the project's `AGENTS.md`.

1. **Plan:** Sol gathers context with Luna and writes all stages, work packets,
   acceptance checks, reviews, and iteration rules. Luna independently reviews
   the plan. Sol presents the numbered revision for your approval.
2. **Approve:** Reply, for example, “Approve plan v01.” Implementation then
   begins within that scope.
3. **Implement and check:** Luna handles assigned tasks. Sol integrates results,
   verifies them, and has another Luna agent review the stage.
4. **Review externally:** Sol gives you a complete Grok 4.7 review packet.
   Submit it and its listed attachments to Grok, then paste the response back
   into the same Codex chat or point Codex to the saved response file.
5. **Iterate:** Sol evaluates the findings, directs repairs, and prepares fresh
   review for changed work. The next stage begins only after this stage passes.
6. **Report:** After all stages and final checks pass, Sol provides deliverables
   and a final report linked to the plan, log, and reviews.

You approve the initial plan and material plan changes. Routine repairs within
approved scope proceed automatically. Your plan approval is always required
before implementation. Grok review is required after every implementation stage.

## What is saved

Each work item gets its own `runs/<unique-run-id>/` folder:

```text
plan.md                     Current versioned plan and approval history
plan-vNN.md                 Earlier reviewed/approved versions when revised
progress.md                 Current checkpoint, assignments, events, decisions
reviews/SNN-rNN-request.md   Exact Grok packet and reviewed artifact manifest
reviews/SNN-rNN-response.md  Actual returned Grok feedback
final-report.md             Completed outcome, checks, reviews, and limitations
```

Resume with “Use $sol-luna-grok to resume run [ID].” Sol reads the saved records
and current artifacts before continuing. Pending approval or review stays
pending across interruptions. A new chat does not automatically continue work
in the background; send a resume request when ready.

## Configuration and portability

- `.codex/config.toml` sets Sol at medium reasoning and Luna subagents at high
  reasoning, with a maximum of three concurrent children.
- `.codex/agents/` defines Luna explorer, worker, and reviewer roles. Exploration
  and review use read-only settings; workers inherit the parent permissions.
- `.agents/skills/sol-luna-grok/` contains the reusable workflow, prompts, and
  record templates. Automatic skill selection remains enabled.
- `AGENTS.md` routes project work into the workflow.

Project configuration is loaded only for trusted projects, and an active
chat's explicit model selection can override defaults. These files do not
change the model of an already-running chat. Verify Sol in the picker. If a
tool cannot select custom roles, the skill instructs explicit Luna spawning.
Unavailable models or tools are reported rather than silently replaced.

To use this in another project, copy the skill folder and merge the relevant
project instructions, model settings, and agent definitions with that project's
existing configuration. Keep that project's other rules and settings intact.
No global Codex settings are changed by this package.

This is an instruction-based workflow with durable records, not a standalone
scheduler or a runtime enforcement engine. Grok review is a manual handoff;
no Grok account, API integration, or automatic submission is configured.

## Source references

Model selection, explicit delegation, standalone custom agent files, and
subagent defaults follow the official [Subagents documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents).
Project defaults and precedence follow [Config basics](https://learn.chatgpt.com/docs/config-file/config-basic).
