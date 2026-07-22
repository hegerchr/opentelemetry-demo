---
name: elastic-upstream-sync
description: >-
  Merges open-telemetry/opentelemetry-demo main into the Elastic fork while
  preserving its Elastic distributions and deployment modes
target: github-copilot
tools: ["read", "search", "edit", "execute"]
disable-model-invocation: true
---

Merge `open-telemetry/opentelemetry-demo` branch `main` into this repository's
`main` branch. Use a merge commit; do not rebase or squash the upstream history.

Before changing files:

- Read `AGENTS.md`, `CONTRIBUTING.md`, and `.github/README.md` completely.
- Add or verify an `upstream` remote pointing exactly to
  `https://github.com/open-telemetry/opentelemetry-demo.git`, fetch
  `upstream/main`, and inspect both sides of every conflict.
- Treat upstream as the source of truth for the demo's architecture, APIs,
  generated files, dependency upgrades, and newly added features. Adapt the
  Elastic integration to those changes instead of restoring obsolete upstream
  code.

Preserve these Elastic-specific capabilities:

- Elastic distributions of the OpenTelemetry agents for the Ad, Fraud
  Detection, Kafka, Cart, Payment, and Recommendation services. Keep the
  corresponding manifests, lockfiles, build files, startup commands, and
  `Dockerfile.elastic` files consistent.
- The Elastic OpenTelemetry Collector distribution and the Elastic collector
  configuration overlays.
- Elastic Docker Compose modes, including Elastic Cloud, self-hosted
  `start-local`, and upstream/no-EDOT mode.
- Elastic Kubernetes Helm values and deployment overlays.
- `demo.sh`, Elastic CI/release/integration-test workflows, and Elastic
  documentation and images.
- Existing upstream services and optional compose layers, including agent,
  chatbot, MCP, observability, extras, and profiling layers.

Resolve conflicts semantically. Do not choose all of `ours` or all of `theirs`.
Start with the current upstream structure and reapply the Elastic behavior in
the appropriate new locations. Regenerate dependency lockfiles and generated
artifacts with the repository's documented tools when their source manifests
change.

Never add credentials or local values from `.env.override`. Keep its committed
placeholder form, and do not expose secret values in output, commits, issue
text, or pull request text.

Before finishing:

- Confirm there are no unmerged paths or conflict markers.
- Review the complete diff against `main` and verify that unrelated
  Elastic-only files were not deleted.
- Run the checks required by `CONTRIBUTING.md`, plus targeted builds or tests
  for every conflicted service. At minimum validate the complete Docker Compose
  configuration used by the Elastic demo.
- Commit the merge with an `Assisted-by: GitHub Copilot` trailer.
- Create a pull request targeting this repository's `main` branch. Use the exact
  title `chore: merge upstream open-telemetry/opentelemetry-demo` and leave the
  pull request body empty. Do not post issue or pull request comments;
  repository policy requires that discussion text be written or approved
  verbatim by a human.
