# Branch Protections & Deployment Triggers

| Repository | Default Branch | Current Protection (assumed) | Required Checks | Deployment Trigger |
| --- | --- | --- | --- | --- |
| saturday_league_football_frontend | `main` | Needs PR review + status checks | `react-ci`, `contracts` (when schema touched) | Vercel auto-deploys on merge to `main`. |
| saturday_league_football | `main` | Enable required reviews & CI | `rails-ci`, `contracts` | Kamal deploy via GitHub Actions/manual trigger. |
| sarradabet | `main` | Require PR review + tests | `monorepo-ci`, `contracts` | Render deploy web service on GitHub webhook. |
| pickup-game-manager | `main` | Require PR review + tests | `rails-ci`, `contracts` | Kamal deploy pipeline or manual `kamal deploy`. |

## Action Items
- Audit GitHub settings to confirm branch protection rules; update to require reusable workflows from `platform/ci`.
- Configure GitHub Environments (`dev`, `staging`, `prod`) with required reviewers and secret scopes.
- Align deployment triggers so staging deploys occur on merge to `develop` (or dedicated branch) prior to production promotion.
- Ensure reusable workflow references in each repo stay pinned to the current branch/tag when cutting releases.
