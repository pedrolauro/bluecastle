---
"@ai-hero/sandcastle": patch
---

Add `kiro()` agent provider for the Kiro CLI (`kiro-cli`). Runs
`kiro-cli chat --agent-engine v3 --output-format stream-json` with the prompt
on stdin, using Kiro's hosted Pro/Pro+/Power backend; `KIRO_API_KEY` is
supplied via `.sandcastle/.env`. The engine is pinned to `v3` because the
default `v2` engine silently ignores `--model` and `--effort`.

`KiroOptions` exposes `effort` (`low` | `medium` | `high` | `xhigh` | `max`),
`agent`, `trustTools` (throws if combined with `dangerouslySkipPermissions`),
`requireMcpStartup` and `env`. The stream-json parser maps text deltas, shell
tool calls (`Bash`), the session id, `runFinished.finalText` and
`runError.message` (auth, invalid model, usage limits, throttling) to
Sandcastle events. Kiro's stderr is redirected to `/tmp/kiro-cli.stderr.log`
in the sandbox so the `runError` message becomes the error detail.

Sessions live in `~/.kiro` inside the sandbox, so the provider sets
`captureSessions: false` and has no `sessionStorage`; `--resume-id` is emitted
when `resumeSession` is passed, which only works while the same container is
alive. Adds a matching "Kiro CLI" template to `sandcastle init`.
