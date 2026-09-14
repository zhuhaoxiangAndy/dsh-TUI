# FORK-NOTES

This is a **fork** of [`ccch1mneyyy/dsh-TUI`](https://github.com/ccch1mneyyy/dsh-TUI), kept for one
purpose: to carry a small, ready-to-review footer change that upstream has not asked for. It is not
an active development fork, and nothing here is meant to diverge from upstream.

## What this fork carries

| | |
|---|---|
| Branch | `feat/statusline-subagent-chip` |
| Base | `main` of upstream at `b246411` (no rebase needed as of the last check) |
| Contents | a footer chip that reports subagent runs: `▸ 2 subagents running` while runs are live, `▸ 5 subagents` once they settle (the created total is kept, matching how the web UI keeps the roster visible) |
| Commits | implementation, regression assertions, bilingual docs, then a labelling refinement |
| Pull request | [#894](https://github.com/ccch1mneyyy/dsh-TUI/pull/894) — **closed by the upstream `pr-gate`** |
| Discussion | the feature request itself is filed as a Discussion in upstream `Ideas` |

## Why the pull request is closed

`dsh-TUI` does not accept unsolicited implementation pull requests. From
[`docs/contributing.md`](https://github.com/ccch1mneyyy/dsh-TUI/blob/main/docs/contributing.md) and
the gate's own comment on #894:

> Only repository write/admin/maintain collaborators, or users listed in
> `.github/APPROVED_CONTRIBUTORS`, may open implementation pull requests. Everyone else's
> implementation PR is closed automatically by `pr-gate`, regardless of size, title, test results, or
> whether a human or an agent wrote it.
>
> A maintainer can reopen the pull request explicitly. Reopening by anyone else is closed again.

So the branch is deliberately left **parked and untouched**: it is the artefact a maintainer would
reopen if they want this implementation. Do not nag; do not open an issue to justify code that
already exists (the gate's comment says so explicitly).

## Maintaining this fork

- **Keep the branch based on upstream `main`.** Fetch upstream and rebase only when upstream moves
  far enough to matter. Keep it linear — no merge commits.
- **Do not force-push once a maintainer shows interest**, and never after a reopen.
- **If upstream implements this first**, delete the branch and this note; the fork has no other
  purpose.
- **Keep the PR body honest.** It states plainly that the change was never compiled locally (a
  Windows Application Control policy blocks the vendored `dsh-std` native binding needed by
  `pnpm install`), so CI is the first full type check.
- **Do not commit anything machine-specific**: no local paths, no credentials, no session data. The
  fork contains source changes and docs only.

## Trying the change locally

The upstream package is prebuilt, so the change can be exercised without building this repo:

1. Install the published plugin into an **isolated** DSH home (so a real `~/.dsh` is untouched):
   set `DSH_HOME` to a scratch directory, then
   `dsh plugin --profile dsh-tui add @deepseek-harness-tui/dsh-tui@0.10.1`.
2. Transpile the one or two changed sources over their compiled counterparts in that isolated copy
   (`src/screens/StatusLine.tsx` → `lib/types/screens/StatusLine.js`, and `src/i18n.ts` →
   `lib/types/i18n.js`).
3. Launch that isolated copy and dispatch two subagents; the footer should show the running count,
   then keep the total once they settle.

Verification performed on the author's machine: a parse-only syntax gate over the changed files, a
line-by-line diff of the transpiled output against the shipped build (a handful of lines changed, no
structural drift), and a real interactive run showing the chip appear, update, and settle.
