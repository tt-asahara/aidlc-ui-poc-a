# A/B/C cloud-task comparison runbook

This comparison is executed by isolated cloud development tasks, not by a
Codex CLI session and not as a claim of complete compatibility with official
host hooks. Each arm uses the same fixture, model/task profile, stage prompts,
approval rules, viewport checks, and evaluation rubric. The only intended
variable is the pinned design treatment.

## Frozen inputs

- Fixture: `fixture.json`.
- Scope: `ui-poc-comparison`, strict guard policy, sensors and summary
  confirmation on, plan approval on, collaborators off.
- One integrated UI unit. No real Ponta API, authentication, payment, customer
  data, external services, or remote browser dependencies.
- Arm A loads only `.agents/skills/frontend-design/SKILL.md` from
  `anthropics/skills@dbd4588f9e1033efb41dad4bef2f7947c8993d44`.
- Arm B loads the same Anthropic skill plus its pinned Matt skill entrypoints.
- Arm C loads the pinned Matt entrypoints plus the historical Impeccable
  frontend-design entrypoint and its seven references; it must not load the
  Anthropic frontend-design skill.

Vendoring is not execution. At rough-mockups and refined-mockups, the task must
read every selected entrypoint and referenced file before producing artifacts,
record path, Git blob SHA, byte count, and stage in `treatment-read-evidence.json`,
and state whether it followed the instructions directly or whether native skill
discovery was unavailable. Missing reads block the arm.

## Stage sequence

1. Verify the branch SHA, clean tree, fixture SHA, treatment lock, installed
   payload bytes, plugin validate/build/disposable compose, and the 35 local
   evidence tests.
2. Run the common requirement and screen/state/AC inventory steps. Do not let
   an arm add or remove required mappings.
3. Run rough-mockups. Produce the official core documents plus self-contained
   HTML/assets and `ui-poc-rough-contract.md`.
4. Stop and show the human: rendered rough screens at both viewports, the
   screen/state/AC table, unanswered decisions, asset manifest, treatment read
   evidence, and checker result. Record the person's real decision through the
   available workflow state mechanism; do not manufacture a ledger entry.
5. Start a fresh cloud task to model session restart. It may read only the
   repository state, audit/state files that actually exist, approved artifacts,
   fixture, and treatment files. Record a `resume-manifest.json` listing every
   input path and SHA-256. Conversation-only context is not an input.
6. Run refined-mockups and stop for human review. Show the rough-to-refined
   delta, both viewport renders, accessibility checklist, interaction states,
   full mapping table, asset hashes, and `ui-poc-refined-contract.md`.
7. Code generation remains blocked until a matching, detached, human-managed
   approval receipt exists for the exact refined contract SHA and mapping list.
   Without it, run the consumer sensor only to demonstrate the expected
   refusal. Never write `approved` to make the gate pass.
8. After a real receipt is available, run plan approval, code generation and
   build/test. Preserve `source-manifest.json`, `traceability.json`,
   `code-generation-plan.md`, `unit-test-instructions.md`, `code-summary.md`,
   implementation/test contracts, screenshots, console output, keyboard and
   responsive checks.
9. For negative controls, copy evidence to an isolated test directory and
   verify rejection for missing approval, changed contract bytes, changed
   asset bytes, mapping drift and path escape. Never alter approved evidence.

## Comparison rubric

Score only after all arms reach the same verified boundary: mapping coverage,
task completion, responsive behavior, keyboard/accessibility behavior, visual
hierarchy, Japanese clarity, state/recovery completeness, implementation
fidelity, defect count, and reviewer revision count. Report unavailable checks
as unavailable, not zero or pass. Cost and duration are contextual metrics and
must not override correctness or approval validity.

## Host-compatibility boundary

Normal Bun engine and plugin authoring commands may validate, build, compose in
a disposable install, compile the graph, inspect state, and run deterministic
sensors. Cloud tasks explicitly manage stage ordering, file reads, task restart
and presentation to the human. They do not reproduce official host session
identity, automatic hook heartbeats, prompt-submit capture, or per-tool audit
hooks. No heartbeat, role identity, approval, or audit event may be synthesized
to fill those gaps.
