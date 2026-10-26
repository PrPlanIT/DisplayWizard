# DisplayWizard — Design

## What it is

DisplayWizard (`dwiz`) manages a virtual display driver on Windows: installing, removing and
reloading the driver and configuring virtual monitors. It is a Go backend with a desktop UI.

## Current architecture

```
Electron shell (ui/desktop) ──spawns──▶ dwiz serve (Go, 127.0.0.1:5757)
        │                                    │
        └── renders React UI (ui/web) ──HTTP──┘
                                             └──▶ engine ──▶ Backend (OS-specific)
```

- **Go backend.** A cobra CLI (`dwiz`) and an HTTP API (`dwiz serve`). The engine executes
  typed operations against an OS-specific `Backend` interface (`src/backend/backend.go`).
  Windows is the only implemented backend; `linux` and `darwin` are placeholders.
- **Desktop shell.** Electron spawns the bundled `dwiz serve --addr 127.0.0.1:5757`, polls it
  until healthy, and loads the React UI. `DWIZ_API` points the shell at an externally run
  backend instead (development).

## Direction

### Native only, for now

DisplayWizard ships as a local desktop application. There is no web-facing or remote mode yet,
and none is added before authentication exists (see *Security invariants*).

### One web-tech UI for every surface

The UI is React rendered in a native webview. The same UI serves three surfaces without a
rewrite:

1. native webview (desktop application),
2. local browser against the loopback API,
3. remote browser, once authentication exists.

Native widget toolkits (e.g. Fyne, Gio) are out: they would force a second UI the moment a
browser surface is added.

### Phases

Nothing web-facing outruns authentication.

| Phase | Scope | State |
|---|---|---|
| A — Windows correctness and security | Per-op elevation; driver state read from the driver store (not the staged file); loopback-only bind; DNS-rebinding `Host` guard; elevated-op result channel | Done |
| B — Single binary | Replace Electron with one Go binary embedding a light webview (e.g. Wails) and running the API in-process; drops the sidecar executable and the spawn-and-health-poll lifecycle. | Planned |
| C — Authentication (hard gate before any remote surface) | Per-run local token → local accounts and SSO/OIDC → TLS, every API endpoint protected, and a consent model for remotely requested elevated operations | Planned |
| D — Linux control (optional) | Windows is the target; this need not land. If it does: control a headless Linux instance's displays with the same commands and API as on Windows, behind the same `Backend` interface. Linux creates displays in software (the display server), so this drives that configuration; it never ships a display driver. | Optional |
| E — Tests | Test harness, pure-logic tests (argument quoting, the `__elevated-op` allowlist), `go test` in CI; remove the stale `isAdmin: true` the desktop shell reports now that the backend runs unelevated | Planned |

## Security invariants

### The API stays loopback-only until it is authenticated

The HTTP API is unauthenticated and can install kernel drivers. Until Phase C ships it **must
never bind a routable address**:

- `dwiz serve` defaults to `--addr 127.0.0.1:5757`. Binding elsewhere requires an explicit
  `--addr` and is unsupported before authentication exists.
- Every request passes `guardLoopbackHost` (`src/api/server.go`): a `Host` header other than
  `localhost`, `127.0.0.1` or `::1` is rejected with `403`. This defeats DNS rebinding, where a
  page in the user's browser resolves an attacker-controlled name to `127.0.0.1` to reach the
  local API.

Loopback binding and the `Host` guard are complementary; neither replaces authentication.

### Least privilege: elevate per operation, not per application

The GUI, the API server and the CLI run as the invoking user. Only the three driver-lifecycle
operations need administrator rights: `ensure_driver_installed`, `ensure_driver_absent` and
`ensure_driver_reloaded`.

- `requireAdmin` (`src/backend/windows/executor.go`) checks the process token
  (`IsElevated`, `src/backend/windows/uac.go`). Already elevated → the operation runs inline.
- Otherwise it relaunches **that one operation** as `dwiz __elevated-op <type> --result-file <tmp>`
  through `ShellExecuteExW` with the `runas` verb (UAC consent), in a hidden window, and waits.
- The hidden `__elevated-op` command accepts only the three driver operations; anything else is
  refused, so the elevated entry point cannot run arbitrary work with an admin token.
- The elevated child writes its result as JSON to the result file (mode `0600`); the parent
  reads it back so the UI shows what actually happened. With no result file, the parent maps
  the outcome: UAC declined, failure, or success from the exit code.

One UAC prompt per privileged operation is intended behaviour, not a defect: the application
never holds an elevated token longer than a single driver operation.
