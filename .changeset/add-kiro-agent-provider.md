---
"@ai-hero/sandcastle": patch
---

Add `kiro()` agent provider for the Kiro CLI (`kiro-cli`). Routes Sandcastle
runs through `kiro-cli chat --no-interactive` using Kiro's hosted Pro/Pro+/Power
backend; `KIRO_API_KEY` is supplied via `.sandcastle/.env`. `KiroOptions` exposes
`agentEngine`, `mode` (KAS-only — throws if combined with any other engine),
`agent`, `trustTools` (throws if combined with `dangerouslySkipPermissions`),
and `requireMcpStartup`. Adds a matching "Kiro CLI" template to `sandcastle init`.

Resume is not supported in this release because `kiro-cli` only persists
sessions in interactive mode (observed 2026-05-27); the headless run is
stateless and `--resume-id` therefore has nothing to attach to. The provider
sets `captureSessions: false` and contains in-code notes pointing at the future
work needed (session file remap host↔sandbox, cwd rewrite analogous to Claude
Code's projectsDir handling).
