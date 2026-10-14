# Formal workflow startup probe

Checked 2026-10-14 for Issue #6 in an isolated directory copied from the
official AI-DLC v2.11.0 Claude projection. This probe did not run a model,
generate mockups, change authentication or security settings, manually invoke
hooks, or synthesize a human turn, approval, heartbeat, or audit event.

## Environment and isolated setup

- Source branch: `integration/aidlc-v2.11.0-ui-poc-a`
- Source commit: `a699de1c28aaee3af4a0bb698b2aa8e40d9f6f5a`
- Node: `v24.19.0`
- Bun: `1.4.2`
- The official `dist/claude` projection was copied to a new `/tmp` directory.
- The validated `ui-poc` Claude projection was composed there with its official
  compose hook. The pinned A frontend-design payload and frozen fixture were
  copied into that isolated project.
- `aidlc engine graph compile --check` exited 0 and help listed the composed
  `ui-poc-comparison` scope.

Representative setup and verification commands:

```sh
cp -a dist/claude/. "$PROBE_DIR/"
AIDLC_PLUGIN_ROOT=/tmp/ui-poc-a-claude-build \
  AIDLC_PROJECT_DIR="$PROBE_DIR" AIDLC_HARNESS_DIR=.claude \
  bun /tmp/ui-poc-a-claude-build/hooks/compose.ts
bun .claude/tools/aidlc.ts engine graph compile --check
```

The compose hook above is the plugin's documented installation operation. No
framework lifecycle hook was invoked manually.

## Initialization result

The retired standalone init flag was not used. The official deterministic
intent-creation route was run:

```sh
bun .claude/tools/aidlc.ts engine intent create \
  --scope ui-poc-comparison \
  --arguments '<synthetic Japanese mobile member-benefits rough-mockup request>' \
  --project-type greenfield
```

Result: exit 0. It created an isolated intent record, completed the three
Initialization stages, selected `ui-poc-comparison`, and positioned the
workflow at `intent-capture`. Effective controls were strict guard policy,
sensors on, learnings on, summary confirmation on, plan approval on, and
collaborators off. The state reported 22 total in-scope stages.

## First `next` result and blocker

```sh
bun .claude/tools/aidlc.ts engine orchestrate next
```

Expected for a fully active official Claude host: a stage directive (directly
or after normal rule delivery) that allows `intent-capture` to be executed.

Actual in this cloud development task: exit 0 with a `print` directive stating
that AI-DLC's Claude hooks were not running and requiring host/project hook
enablement before retrying. It did not return `run-stage`.

Read-only doctor evidence:

- Project files and hook registrations were present and intact.
- Resolved `disableAllHooks` was not true.
- Failure: hooks had never executed although the workflow had progressed four
  stage positions (Initialization plus the current stage start).
- Human-turn receipts: 0.
- `runtime-graph.json`: missing, with the doctor linking this to hooks not
  running in this environment.
- No hook drops were recorded.
- Framework schema, graph references, cycles, stages and scopes passed their
  integrity checks.

## Conclusion and scope impact

The deterministic engine can compose the plugin, compile the graph and create
the official workflow state in this cloud task. It cannot honestly advance the
formal host-driven workflow because this task is not a Claude host and does not
emit SessionStart, UserPromptSubmit, PreToolUse, PostToolUse or Stop hook
events. Running those hook scripts by hand or writing settings/receipts would
invalidate the evidence, so neither was done.

This blocks Issue #6 from claiming formal stage execution in the current
environment and consequently blocks Issues #7 and #8. It does not implicate
the UI plugin or require a source-code fix. Issue #5's Codex compose mismatch
is separate and remains non-blocking for this startup result.

Two honest paths remain:

1. Run the official projection inside a supported host that actually dispatches
   its hooks, with the person's normal trust/setting approval; or
2. Continue the documented cloud-task operational pilot with explicit stage
   management, clearly classified as not fully official-host-compatible.

No normal approval ledger exists, so even either path must stop before a
consumer stage until a real human-managed receipt binds the approved artifact.
