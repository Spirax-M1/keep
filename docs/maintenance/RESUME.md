# Site24x7 contribution review

2026-11-01. This repository is a fork of keephq/keep. Preserve the upstream README, license and attribution. The user's contribution is upstream PR https://github.com/keephq/keep/pull/6201 on branch `fix/site24x7-status-and-tags`; it was open when reviewed.

This follow-up uses `maintenance/site24x7-input-validation`, based on that contribution branch. It must be reviewed against `fix/site24x7-status-and-tags`, not main. Do not update the upstream PR or submit messages to upstream maintainers without authorization.

## Findings and change

The existing formatter passed dictionary tags directly to AlertDto and emitted an empty label name for inputs such as `:value`. Tag parsing now lives in a dependency-free helper, copies dictionary inputs and ignores empty keys. Valid existing key/value and bare tags retain their behavior, including first-colon splitting and last-value-wins duplicate names. The mapping copy is shallow; nested values are not modified or deep-copied.

Ten focused unittest cases cover these behaviors. Run:

```sh
python -m unittest discover -s tests/providers/site24x7_provider -p test_tag_parser.py -v
```

The added workflow checks Python 3.11–3.13. These checks validate parsing only, not the full provider, AlertDto validation or external Site24x7 requests.

## Full integration checks still needed

The upstream PR showed title, issue-link and security checks, but no completed provider unit-test run was visible. The original 16 provider tests, plus two new formatter-level edge cases in this revision, require the full Keep environment. This PC has Python 3.14, outside the supported >=3.11,<3.14 range, and lacks pytest and several service dependencies. Do not claim those 18 formatter tests have passed in this review. No Actions run was visible for the new parser workflow immediately after the fork branch push; verify workflow availability in the fork before assuming CI coverage.

In a compatible development environment, install the locked project dependencies and run the existing provider tests. Confirm real webhook payload examples and check mapping/status behavior with AlertDto before proposing the follow-up upstream. The main branch belongs to the upstream application; it has not been rewritten as an original portfolio project.

## CI follow-up

The ten parser tests passed on Python 3.11, 3.12 and 3.13. The inherited Unit Tests workflow stops at a repository-wide Ruff check (3,482 diagnostics), before installing dependencies or executing provider tests; diagnostics include unchanged upstream files. The inherited PR policy also requires a linked issue. The follow-up therefore adds a dedicated Python 3.11 formatter job using the real locked Keep dependencies and the complete 18-case formatter suite. Check that job separately; do not describe the entire upstream application CI as passing.
