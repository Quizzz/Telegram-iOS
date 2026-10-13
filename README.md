# Telegram-iOS fork automation

This branch only holds the automation for the fork. The code is in `master` and the feature branches.

## Branches

- `master` is an exact mirror of [TelegramMessenger/Telegram-iOS](https://github.com/TelegramMessenger/Telegram-iOS) `master`. Never commit to it.
- Every other branch is a feature branch built on top of `master`. One branch per pull request, squashed to one commit, as [CONTRIBUTING.md](https://github.com/TelegramMessenger/Telegram-iOS/blob/master/.github/CONTRIBUTING.md) asks.
- `fork-automation` is this branch, the default one, so that scheduled workflows can run.

## What `sync-upstream.yml` does (every 6 hours, or manually from the Actions tab)

1. Fast-forwards `master` to upstream.
2. Rebases each feature branch onto the new `master` and force-pushes it with a lease. If there is a conflict, the branch is left as is, the run fails and the branch head gets a red `fork-ci/upstream-sync` status.
3. Builds each branch head that hasn't been built yet for the iOS simulator (`debug_sim_arm64`, fake codesigning, placeholder API keys), and reports a `fork-ci/simulator-build` status on the commit.

Because the bot rewrites feature branches, update your local copy with:

```bash
git pull --rebase
```

## If upstream changes its workflow files

`GITHUB_TOKEN` cannot push commits that touch `.github/workflows`. If the sync fails for that reason, add a classic personal access token with `repo` and `workflow` scopes as the repository secret `SYNC_TOKEN`.
