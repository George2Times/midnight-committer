# Fleet Notes

## Must-have: "fix or retire the auto-committer" — investigated, no action needed

The task described this repo's daily auto-commit workflow as "silently
broken since roughly Feb 2025," pointing to `daily-log.txt` having only
2 entries and assuming the PAT had expired.

That was true of the **local clone** in this workspace, but not of the
actual repository on GitHub. I verified via `gh run list` and
`gh api repos/.../commits` that `.github/workflows/main.yml` has been
running successfully every single day without interruption, right up
through today (2026-07-12) — `daily-log.txt` on `origin/master` has 526
entries, one per day since inception, and the workflow's last run
(2026-07-12T03:31:01Z) committed and pushed cleanly. The `PAT_TOKEN`
secret is valid; nothing needed regenerating.

Root cause of the confusion: this local clone was never pulled after
its initial 2025-02-04 commits, so it looked frozen/dead. Local
`git fetch`/`pull` was additionally failing here with an SSL cert
error until `git config --global http.sslbackend schannel` was set
(Git for Windows' bundled CA bundle wasn't resolving GitHub's cert in
this sandbox; the OS cert store via schannel works). I've applied that
config and fast-forwarded local `master` to `origin/master` (524
commits, no local-only work existed, so this was a pure sync — no
history was rewritten or discarded).

**Decision:** retire nothing, change nothing in the workflow. It is
doing its job. No commit was needed for this item beyond this note and
the local sync above.

If you (human) independently observe the workflow actually failing
going forward, re-check `gh run list --workflow=main.yml` on the real
repo rather than trusting a possibly-stale local clone.
