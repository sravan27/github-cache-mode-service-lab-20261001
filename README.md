# GitHub Actions cache-mode service-enforcement lab

Owned synthetic test for the generally available `cache-mode` control.
No secrets or third-party repositories are involved.

Hypothesis: a step-level `ACTIONS_CACHE_MODE=write` override may cause
`actions/cache/save` to attempt a save even when the job explicitly declares
`cache-mode: read`. GitHub documents that the cache service enforces the
job's scoped token; therefore, the save must still be denied.

The four manually dispatched workflows use unique, fixed synthetic keys:

- `cache-read.yml`: read-only negative control, save skipped.
- `cache-spoof.yml`: read-only candidate with only the step environment spoofed.
- `cache-write.yml`: write-capable positive control, cache save succeeds.
- `cache-verify.yml`: independent read-only restore of the candidate and
  positive-control keys.

A reportable result would require the read-only candidate key to be saved by
the cache service and independently restored with the exact marker, while the
negative control remains absent and the positive control succeeds.

Sources:

- https://github.blog/changelog/2026-09-10-control-github-actions-cache-access-with-cache-mode/
- https://docs.github.com/en/actions/reference/workflows-and-actions/dependency-caching
- https://hackerone.com/github?type=team
