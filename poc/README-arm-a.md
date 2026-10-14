# AI-DLC UI PoC A

Arm A uses Anthropic `frontend-design` in the isolated A/B/C UI comparison.

The repository root is the unmodified official AI-DLC v2.11.0 source tree. The
A-specific integration lives under this `poc/` path so the official core is not
changed:

- `arm.json` selects treatment A through `ui-poc/treatments/a.json`. This is
  selection metadata, not an upstream runtime configuration API.
- `ui-poc/` contains the shared plugin source, tests, provenance records,
  evidence contract, and all treatment definitions. Its plugin directory and
  manifest identity are both `ui-poc`.
- `../WORKLOG.md` records engineering decisions, verification outcomes, known
  blockers, and publication checkpoints.

The shared plugin is intended to remain byte-identical across A/B/C. Only arm
selection and repository-level documentation should differ. All treatment
pins, including C's historical official component, remain in the shared source
lock.

## Local verification

From this repository:

```sh
cd poc/ui-poc
node --test tests/evidence.test.cjs
python3 tests/check-source-contract.py --source-root ../.. --tree /path/to/pinned-tree.json
```

The Node suite requires no dependency installation. The static source-contract
check requires a complete tree JSON for the pinned source. These checks do not
establish successful official validation/build/compose, live treatment
invocation, human approval capture, or browser evaluation.

## Branch workflow

The official v2.11.0 commit is retained unchanged on
`baseline/aidlc-v2.11.0`. The integration branch merges the original A `main`,
that official commit and `feature/ui-poc-foundation`, preserving all three
histories. `main` remains unchanged pending review, and the existing Draft PR
#1 remains open.

See [the plugin README](ui-poc/README.md),
[verification record](ui-poc/docs/verification.md), and
[engineering work log](../WORKLOG.md) before installation or live comparison.
Missing trusted evidence must block execution; synthetic fixtures are not
approvals or evaluation results.
