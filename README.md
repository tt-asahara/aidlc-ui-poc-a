# AI-DLC UI PoC A

Arm A: Anthropic frontend-design.

This repository contains an **initial integration scaffold** for the AI-DLC UI comparison. It is not a full upstream checkout or imported upstream history, and it does not establish live A/B/C success.

## Contents and arm selection

- `poc/ui-poc/`: shared plugin source, tests, source provenance, evidence-contract documentation, and all treatment definitions. Plugin directory and manifest identity are both `ui-poc`.
- `poc/arm.json`: explicit arm A selection, referring to `poc/ui-poc/treatments/a.json`. Its relative paths resolve from the `poc/` directory. This is selection metadata, not an upstream runtime configuration API; no runtime is installed or activated by this file.
- `WORKLOG.md`: dated engineering decisions, verification outcomes, review fixes, known blockers, and publication checkpoints.

The shared plugin is intended to remain byte-identical across A/B/C. This checkpoint publishes A first; B/C remain unmodified until their pristine upstream baseline is recorded separately. Only arm selection and repository-level documentation should differ. All treatment pins, including C's historical official component, are preserved in the shared source lock.

## Local verification

From this repository:

```sh
cd poc/ui-poc
node --test tests/evidence.test.cjs
python3 tests/check-source-contract.py --source-root /path/to/pinned-source --tree /path/to/pinned-tree.json
```

The Node tests require no dependency installation. The static source-contract check requires separately obtained, inspected source files and the complete pinned Git tree. The recorded checkpoint passed 35 local synthetic tests and the narrow static source check; full upstream build/compose, runtime gate behavior, actual skill invocation, human approval capture, and browser evaluation remain unverified.

## Provenance and next steps

The official upstream reference is AI-DLC v2.11.0, commit `6a378b53c0a4fe0641ed7d8de8dfff94264d5b6a`. It is provenance only at this stage. No full upstream checkout/history, automatic CI workflow, package hook, or deployment is included.

Read [the plugin README](poc/ui-poc/README.md), [verification record](poc/ui-poc/docs/verification.md), and [engineering work log](WORKLOG.md) before proceeding with installation or live comparison. Missing trusted evidence must block execution; synthetic fixtures are not approvals or evaluation results.

## Branch workflow

`main` currently retains the initial A README bootstrap; it is not claimed to be a pristine upstream fork. Work is committed on `feature/ui-poc-foundation` and proposed through a draft pull request. No unverified implementation is merged into `main`. Future changes should use focused feature branches and reviewable commits.
