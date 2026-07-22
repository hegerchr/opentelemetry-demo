# Automated upstream synchronization

The `Merge Upstream PR` workflow checks `open-telemetry/opentelemetry-demo`
twice a week and on manual dispatch. When new commits exist and no sync is
already open, it starts the `elastic-upstream-sync` Copilot agent. Copilot
performs the merge, resolves conflicts according to the agent profile, and
opens a pull request against this repository's `main` branch.

## Required setup

1. Enable GitHub Copilot cloud agent for the repository and the user represented
   by the token. Starting tasks through the API requires Copilot Business or
   Copilot Enterprise.
2. Create a fine-grained personal access token with read/write access to the
   repository's Agent tasks permission.
3. Store that token as the repository Actions secret
   `COPILOT_AGENT_TOKEN`.
4. Review and approve the exact task prompt in `merge-upstream.yaml` before
   enabling the schedule. Repository policy does not allow an AI agent to author
   issue or pull request discussion text.

The token must be a user-to-server token. The built-in `GITHUB_TOKEN` and GitHub
App installation tokens cannot start Copilot cloud agent tasks.

The workflow does not start another sync while any Copilot task is active in the
repository or a pull request with the title
`chore: merge upstream open-telemetry/opentelemetry-demo` is open.
