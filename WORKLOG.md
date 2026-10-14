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

A was bootstrapped on main with README commit [3c8c0c5237de5dda77489e6a83de2fb32d78a51c](https://github.com/tt-asahara/aidlc-ui-poc-a/commit/3c8c0c5237de5dda77489e6a83de2fb32d78a51c), and its remote main ref was verified. That commit is a repository bootstrap, not a pristine upstream fork. This integration scaffold is published on feature/ui-poc-foundation for a draft pull request, retaining main unchanged. B/C have not been modified; their pristine upstream base must be recorded before treatment changes. No CI workflow, package lifecycle hook, installation, or automatic deployment is added. 

Remote publication verified on 2026-10-14:

- Scaffold commit: [d8e9d48c7804fd6a3b27c751c5f9b48eae7485f2](https://github.com/tt-asahara/aidlc-ui-poc-a/commit/d8e9d48c7804fd6a3b27c751c5f9b48eae7485f2).
- Its parent is README bootstrap 3c8c0c5237de5dda77489e6a83de2fb32d78a51c; its Git tree is 288c60e140e522280de04feea1d4a01f3b39f3cb.
- The remote feature branch points to that scaffold commit. The recursive remote tree is complete; all 47 file blob hashes match the tested local snapshot.
- [Draft PR #1](https://github.com/tt-asahara/aidlc-ui-poc-a/pull/1) proposes feature/ui-poc-foundation into main, without a merge.
- GitHub Actions reported 0 workflow runs and 0 check runs for that exact commit; the commit-status API returned no statuses. This records absence of CI results, not a passed CI claim.
- The separate documentation checkpoint containing this record keeps the shared plugin code unchanged. Future implementation increments should rerun relevant tests before pushing and verify the exact remote commit afterward.



## 2026-10-14 — Complete upstream baseline and runtime preflight

This later checkpoint supersedes the earlier selected-source availability limitation. It does not import upstream history into A or report a successful live workflow.

- Obtained a complete, non-shallow checkout of official [AI-DLC v2.11.0](https://github.com/awslabs/aidlc-workflows/tree/6a378b53c0a4fe0641ed7d8de8dfff94264d5b6a): commit `6a378b53c0a4fe0641ed7d8de8dfff94264d5b6a`, tree `6697c147c055206ac67ca69c91da83c53b2b0cb2`, annotated tag object `4079edbea52cf4d3c80e580825da3a4dd8213271`.
- Verified 1,130 reachable commits (including HEAD), 1,673 tracked files, no submodule entries, and a clean working tree. `git fsck --full --no-reflogs` completed successfully; `git rev-parse --is-shallow-repository` returned `false`. The complete baseline remains separate from this scaffold.
- Repeated the source-contract check against the complete pinned source: 23 contribution targets/anchors and all 1,673 source blob identities passed, with projected runtime documentation/config copies matching. Earlier entries accurately record the initial 30-file subset check. Neither check substitutes for the official validator or composer.
- The unchanged checker suite again passed 35 tests, with 0 failures and 0 skipped tests. This update changes documentation only.

### Official validator runtime blocker

- Bun is absent, so the official plugin validator could not complete. A Node v24.19.0 attempt failed with `ERR_UNSUPPORTED_TYPESCRIPT_SYNTAX` on a TypeScript constructor parameter property. A second attempt using `--experimental-transform-types` progressed further but failed with `ERR_UNSUPPORTED_ESM_URL_SCHEME` for the Bun-specific `bun:` import. These are blocked runtime attempts, not a plugin-validation result.
- Bun 1.4.2 matches the pinned upstream [primary CI workflow](https://github.com/awslabs/aidlc-workflows/blob/6a378b53c0a4fe0641ed7d8de8dfff94264d5b6a/.github/workflows/ci.yml). Its [official release metadata](https://github.com/oven-sh/bun/releases/tag/bun-v1.4.2) was inspected; the binary has not been downloaded, verified, or installed.
- A dedicated cloud runtime is requested but not yet available. Official validation/build/compose and subsequent integration checks remain pending there.

### Baseline publication blocker

- B/C remain unmodified. Their pristine official baseline must be pushed before separate feature-branch changes so upstream-versus-treatment changes remain reviewable.
- An authenticated Git transport is not yet configured for the full-history push. Standard GitHub CLI OAuth requests broader scope than this narrow operation, and safe credential storage is unverified; no login flow was started.
- Next: establish a suitable authenticated transport and dedicated runtime, verify pristine B/C remote commits/trees first, then create focused feature branches and draft PRs for treatment changes. No merge or successful live A/B/C execution is claimed.


## 2026-10-14 — Cloud checkout and access-layer diagnosis

- Created a saved cloud environment configuration for the project. Its first execution attempt stopped during initial repository checkout with a repository-not-found/access error. None of the intended Node, Bun, or Codex checks, or B/C tests, ran in that task.
- Separately, authenticated repository API access remained functional: A could be read, and both `main` and `feature/ui-poc-foundation` matched their recorded remote commits. API access and the cloud checkout path are separate access layers; success in one does not establish access through the other.
- Subsequently found an updated saved environment configuration. A second launch request returned a generic backend error; no new execution task was confirmed. This does not establish a successful launch or runtime availability.
- Rechecked A's existing two branches and PR discussion state before proposing a public-visibility diagnostic. No additional branch or PR discussion content was found at that checkpoint. The visibility diagnostic has not been performed, and A remains private.
- The checkout failure's underlying cause is unverified. Neither repository code correctness nor a specific credential/configuration defect can be inferred from the access error alone.
- Next: establish and verify a working checkout path, then resume the pending runtime checks and pristine B/C baseline publication. B/C remain unmodified by this diagnosis. This is an access-diagnosis record, not an A/B/C experiment result; no merge or visibility change is included.
