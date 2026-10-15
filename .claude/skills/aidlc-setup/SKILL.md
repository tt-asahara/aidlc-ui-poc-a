---
name: aidlc-setup
description: Set up this repository's AI-DLC Claude runtime and isolated Arm A UI comparison plugin in another product repository. Use when a user asks to preview or apply AI-DLC A to a product checkout while preserving its existing files, settings, plugins, and uncommitted work.
argument-hint: "[A checkout path] [product repository path]"
user-invocable: true
---

# Set up AI-DLC A

Invoke this skill from a Claude Code project rooted at the A checkout that contains it. Treat setup as a reviewable transfer between two checkouts, not as permission to modify a product merely because this skill was invoked.

## Establish boundaries

1. Resolve two absolute paths from the arguments or ask for them:
   - `SRC`: this A repository checkout.
   - `PRODUCT`: the target product repository.
2. Confirm the intended target, project-local installation scope, trust/permission choices, and whether the user currently authorizes only a dry run or also authorizes applying the reviewed plan. Prefer project scope. Do not change authentication.
3. Inspect `git -C "$SRC" status --short --branch` and `git -C "$PRODUCT" status --short --branch`. Preserve all tracked, untracked, ignored, and uncommitted product files. Do not replace existing plugin selection or unrelated settings.
4. Read these source contracts before proceeding:
   - [`../../../poc/ui-poc/README.md`](../../../poc/ui-poc/README.md)
   - [`../../../poc/ui-poc/docs/verification.md`](../../../poc/ui-poc/docs/verification.md)
   - [`../../../poc/ui-poc/docs/contract.md`](../../../poc/ui-poc/docs/contract.md)
   - [`../../../poc/ui-poc/treatments/a.json`](../../../poc/ui-poc/treatments/a.json)

Do not assume an existing product is greenfield. Ask when project type is not established.

## Build the local projections

Run from `SRC`, stopping on any failure:

```bash
bun install --frozen-lockfile
bun scripts/package.ts claude
bun dist/claude/.claude/tools/aidlc-plugin-build.ts poc/ui-poc claude --json
```

The runtime source is `SRC/dist/claude`. The plugin marketplace root is `SRC/poc/ui-poc/dist/claude`. Do not pass `dist` or `.claude` itself to `--from`, and do not use `--force` to hide a collision.

## Review the runtime transaction

Run the supported dry run first:

```bash
bun "$SRC/dist/claude/.claude/tools/aidlc.ts" config --from "$SRC/dist/claude" --harness claude --project-dir "$PRODUCT" --mcp none --dry-run
```

Show the planned writes and collisions. Stop for review if the product is dirty, a planned path already exists with different content, or the plan would disturb settings, hooks, skills, or other plugins. A successful dry run is not authorization to apply.

Only after the user explicitly authorizes the reviewed target changes, rerun the same command without `--dry-run`. Do not infer this authorization from a request to create, inspect, or invoke this setup skill.

## Place the Arm A design skill without clobbering

The plugin does not bundle `frontend-design`. Compare `SRC/.agents/skills/frontend-design` with `PRODUCT/.agents/skills/frontend-design`.

- If the target is absent and the user authorized this product write, copy the complete directory to the same relative path.
- If it is byte-identical, leave it unchanged.
- If it differs, stop and ask how to resolve the collision. Never overwrite it unconditionally.

Verify that the installed `SKILL.md` and `LICENSE.txt` match the Arm A inventory in `poc/ui-poc/treatments/a.json` and its adjacent `source-lock.json`.

## Install the Claude plugin

Before leaving the A-checkout session, give the user a short handoff containing the resolved absolute `SRC` and `PRODUCT` paths, the exact plugin commands below with `SRC` substituted, and the first verification commands. Do not treat `cd "$PRODUCT"` in a shell tool as changing Claude Code's project.

Open a new Claude Code session with `PRODUCT` as its project/workspace root. In that new session, verify that the host's project root and `git rev-parse --show-toplevel` resolve to `PRODUCT`; stop if either points elsewhere. Continue the handoff from this section even though the source-only `/aidlc-setup` skill is no longer discovered there.

After confirming the marketplace path, project-local scope, and requested trust/permissions in the PRODUCT-rooted session, run:

```text
/plugin marketplace add <SRC>/poc/ui-poc/dist/claude
/plugin install aidlc-ui-poc@aidlc-plugins
```

Keep all existing plugin selections. Start another PRODUCT-rooted Claude Code session so the composition hook runs; recheck the project root, then open `/plugin` and inspect its **Installed** tab (or run `claude plugin list` in a shell), followed by `/aidlc --doctor`. This is Claude Code's installed-plugin check, not an AI-DLC engine `plugin list` command. Confirm that `ui-poc-comparison` is present and review every compose drop or diagnostic before continuing.

## Stop at rough review by default

Before starting, explicitly record that this is Arm A and read the installed copies of:

- `.claude/tools/ui-poc/treatments/a.json`
- `.agents/skills/frontend-design/SKILL.md`
- `.agents/skills/frontend-design/LICENSE.txt`

Invoke:

```text
/aidlc --scope ui-poc-comparison
```

Preserve brownfield/greenfield classification as decided by the user. Stop after the rough-mockups review unless the user separately authorizes later lifecycle work.

Never create, amend, or fabricate `UI_POC_APPROVAL_LEDGER` entries. Code generation requires a real approval recorded in an independently managed ledger. Treat formal Claude host-hook behavior, ledger independence/ACLs, and a complete real-model pass as unverified until separately demonstrated. The plugin affects contributed target stages outside its owned scope while enabled, so use it only in an isolated comparison repository and do not claim stock-scope isolation.

## Report

Separate successful checks from unverified conditions. Report the resolved paths, build results, dry-run plan, user-authorized writes, preserved collisions/settings/plugins, frontend-design disposition, plugin/doctor/scope checks, and the exact stop point. Never describe a dry run, source build, synthetic test, or rough draft as a completed live workflow or human approval.
