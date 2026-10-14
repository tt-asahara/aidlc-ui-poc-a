# Experiment preparation verification

Checked 2026-10-14 with official AI-DLC v2.11.0 and Bun 1.4.2. This is cloud
development task evidence, not a model-backed experiment result and not a
complete official-host compatibility result.

## Commands and results

```sh
bun core/tools/aidlc-plugin-validate.ts poc/ui-poc --json
bun scripts/package.ts
bun scripts/package.ts --check
bun dist/claude/.claude/tools/aidlc-plugin-build.ts poc/ui-poc claude /tmp/ui-poc-a-claude-build --json
bun dist/claude/.claude/tools/aidlc-plugin-test.ts poc/ui-poc --install dist/claude --harness claude --json
node --test poc/ui-poc/tests/evidence.test.cjs
```

After adding non-empty scope keywords: validate passed; packaging was
deterministic; Claude build and disposable compose passed with no drops, graph
scope present, and idempotent second compose; 35 dependency-free evidence tests
passed.

The standalone builder must be run from a packaged tool bundle. Running
`bun core/tools/aidlc-plugin-build.ts ...` directly failed because authored
`core/tools/data/plugin-targets.json` does not exist; the packaged bundle
contains the generated target table.

## Reproduced official Codex compose mismatch

```sh
bun dist/codex/.codex/tools/aidlc-plugin-test.ts \
  poc/ui-poc --install dist/codex --harness codex --json
```

Expected: no compose drops and `idempotent: true`.

Actual on v2.11.0: generated Codex skills are under `.agents/skills`, while the
compose hook looked for `.codex/skills`; it recorded
`runner regeneration skipped: .codex/skills not present in this install`.
The second compose removed the first advisory drop file, so the test also
reported `idempotent: false`. Official core and composer were not changed.

An official integration control also reproduced a pre-existing mismatch:

```sh
bun test tests/integration/t188-plugin-compose.serial.test.ts \
  -t 'pre-sidecar plugin knowledge ownership is backfilled'
```

Expected directive kind: `run-stage`. Actual: `print` (1 failed test). This is
not attributed to the UI plugin and is not counted as a passing integration.

## Approval boundary

No normal approval ledger is present. Rough/refined producer shape checks can
run, but code-generation consumption must refuse until the human reviews the
exact refined artifact and an operator-managed detached receipt binds its hash
and required mapping order. Synthetic unit-test ledgers demonstrate checker
behavior only and are never experiment approvals.
