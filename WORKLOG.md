# Engineering work log

## 2026-10-14 — Initial integration foundation

### Scope and design decisions

- Prepare isolated A/B/C comparison repositories with identical shared `poc/ui-poc/` source. `poc/arm.json` records the intended arm and treatment config, without claiming runtime activation.
- Retain core AI-DLC Markdown deliverables and add plugin-prefixed UI evidence contracts for rough mockups, refined mockups, implementation, and testing. HTML alone does not establish a handoff.
- Pin AI-DLC v2.11.0 to [6a378b53c0a4fe0641ed7d8de8dfff94264d5b6a](https://github.com/awslabs/aidlc-workflows/commit/6a378b53c0a4fe0641ed7d8de8dfff94264d5b6a). This is provenance only; no complete upstream tree or history is imported in this checkpoint.
- Pin all treatment sources in [source-lock.json](poc/ui-poc/treatments/source-lock.json). C uses the historical official component at [209444a9d552c18bcaa74e26bf66a25fc568a767](https://github.com/pbakaus/impeccable/commit/209444a9d552c18bcaa74e26bf66a25fc568a767), with seven references, conditional teach-impeccable dependency, and license/notice inventory. The latest full runtime/hooks and historical critique script are excluded.
- Do not replace the ordinary human stage approval with a synthetic receipt. Consuming stages require an independently managed approval ledger binding the preceding contract hash and mapping inventory. No production ledger or approvals are supplied.

### Review findings and fixes included

- A metadata-only approved implementation envelope could otherwise hide missing predecessor content. Validation now recursively checks the refined → implementation → testing chain, including ancestor headings, assets, hashes, mappings, and ordinary prior-stage approval receipts.
- Bound chain depth and reject repeated real file identities; reject traversal, absolute paths, symlink escapes, changed upstream bytes, incomplete/duplicate mapping coverage, and wrong-stage/wrong-arm receipts.
- Resolve the stage-specific sibling contract when dispatch fires for another deliverable; draft producers do not gain an extra approval gate.
- Project runtime contract/config copies under the supported `tools/ui-poc/` subtree, and verify byte identity with source copies.
- Require the pinned commit and a complete Git tree for static source checks. Preserve the plugin directory/manifest identity `ui-poc`.
- Document that contributions affect target stages whenever enabled; scope membership alone does not isolate behavior. Use isolated comparison repositories and disable the plugin for unrelated workflows.

### Verification at this checkpoint

Executed using installed Node v24.19.0 and Python 3, without dependency installation:

- `node --test tests/evidence.test.cjs`: 35 passed, 0 failed, 0 skipped. These are synthetic local checker tests, including the actual TypeScript adapter on missing input.
- `python3 tests/check-source-contract.py --source-root <pinned-source> --tree <pinned-tree.json>`: 23 contribution targets/anchors and 30 selected source blob identities passed; projected runtime docs/config copies matched.
- Exact test output: [local-test-results.txt](poc/ui-poc/docs/local-test-results.txt). Source identities and recorded newline materialization: [source-evidence.json](poc/ui-poc/docs/source-evidence.json).

### Failed attempts, unrun checks, and next steps

- Live CLI preflight is blocked by a read-only home directory. No permission or security workaround was made, and no live stage handoff was run.
- Official plugin validation/build/compose, graph inspection, idempotent second compose, real dispatcher/audit/blocking gate integration, and actual treatment reads remain NOT RUN. The inspected selected source set is not a complete buildable upstream checkout.
- Trusted human approval capture, ledger ownership/ACL enforcement, actual reviewer identity, multi-unit aggregation, and browser behavior/visual/accessibility quality remain unverified.
- First obtain an authorized complete pinned build/runtime, validate the plugin and compose behavior, then verify trusted approval capture and perform comparable live A/B/C lifecycle runs with measured outcomes.
- The former custom stage-2.5 extension is unavailable; exact reproduction is not claimed. The present source-pinned integration can be evaluated independently.

See [verification.md](poc/ui-poc/docs/verification.md) and [contract.md](poc/ui-poc/docs/contract.md) for details. No result here establishes A/B/C superiority, upstream integration success, or human artifact approval.

### Version-control checkpoint

A was bootstrapped on main with README commit [3c8c0c5237de5dda77489e6a83de2fb32d78a51c](https://github.com/tt-asahara/aidlc-ui-poc-a/commit/3c8c0c5237de5dda77489e6a83de2fb32d78a51c), and its remote main ref was verified. That commit is a repository bootstrap, not a pristine upstream fork. This integration scaffold is published on feature/ui-poc-foundation for a draft pull request, retaining main unchanged. B/C have not been modified; their pristine upstream base must be recorded before treatment changes. No CI workflow, package lifecycle hook, installation, or automatic deployment is added. Exact scaffold remote verification will be recorded after publication.
