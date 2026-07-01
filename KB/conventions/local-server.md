# Local Server Convention

Generic HTTP server shared across all projects — file access, static serving, Forge proxy, and process execution.

*Document type: Convention — see `conventions/documentation-style.md`*

## Quick Start

Convention for the shared local development server (`tools/local-server.js`).
Covers: why a local server is needed, the server model (allowed roots, pure HTTP),
the full API contract, how projects use it, the Forge v2 proxy, and the process execution model (exec sessions).
Load when implementing or modifying the server, writing project HTML pages that use it,
or setting up a new project that needs local file access, Forge access, or terminal execution.
Does not cover project-specific logic — that belongs in the project's own spec.

## Load when
Setting up or using the local development server


```insta-toc
---
title:
  name:
  level:
  center:
exclude:
style:
  listType:
omit:
levels:
  min:
  max:
---

# Table of Contents

- Local Server Convention
    - Quick Start
    - Keywords
    - Why a local server
    - Model
        - Pure HTTP server
        - Allowed roots
        - URL scheme for static files
        - Forge proxy
        - Process execution
    - Startup
        - Command
        - Batch file
        - Confirmation output
        - Port conflict
    - API Contract
        - GET /ping
        - GET /file
        - POST /file
        - DELETE /file
        - GET /dir
        - POST /forge
        - POST /forge-reload
        - POST /exec
        - GET /exec/stream
        - DELETE /exec
        - GET /
        - OPTIONS (preflight CORS)
    - Session model
        - What a session is
        - Session lifecycle
        - Inactivity timeout
        - Session map
    - How projects use the server
        - Bootstrap file
        - Building API paths in pages
        - Using exec sessions
        - Availability indicator
    - Security
        - Path traversal prevention
        - Process execution scope
        - Local only
        - No authentication
    - What the server does not do
    - Index
    - Changelog
        - Version 1.1 - Forge proxy and exec session endpoints added
        - Version 1.0 - Creation
```

## Why a local server

Browsers block local file access from HTML pages opened via `file://` protocol:
- `fetch()` calls to other local files fail (CORS / same-origin restrictions)
- There is no way to write files from the browser

A local HTTP server solves both problems: all pages are served from the same origin
(`http://localhost:3000`), fetch works normally, and the server exposes write endpoints.

A **single shared server** is preferable to one server per project:
- One process to start at laptop boot, not one per project
- No port conflicts when working on multiple projects in parallel
- The server is generic — no project-specific knowledge embedded in it

## Model

### Pure HTTP server

The server is a **pure infrastructure layer**. It knows nothing about artifacts,
revisions, JSON schemas, or business rules. It reads, writes, deletes, and lists files,
proxies calls to the Forge MCP server, and spawns child processes on demand.

Project-specific logic (which files to fetch, how to name them, what to validate)
lives in the HTML pages and project scripts — not in the server.

### Allowed roots

The server restricts file access to a configurable list of **allowed roots** —
absolute paths to directories the server is permitted to read and write.

Any request targeting a path outside all allowed roots is rejected with `403 Forbidden`.

Allowed roots are passed as command-line arguments at startup:

```
node tools/local-server.js "C:/Users/Remi/Dropbox" "C:/Users/Remi/GoogleDrive" "D:/Projects"
```

This mirrors the MCP filesystem server model — explicit allowlist, no implicit access.

### URL scheme for static files

Projects are accessed via their absolute path encoded in the URL:

```
http://localhost:3000/C:/Users/Remi/Dropbox/AfrSCM/AssistantIA/reunions/index.html
```

The browser resolves all relative `fetch()` calls from this page using the same base URL.
No path configuration is needed in the HTML pages themselves — standard browser URL resolution handles it.

### Forge proxy

The server embeds a Forge v2 proxy (`/forge`) that dispatches tool calls to the Forge MCP server
running in the same process. The Forge instance is initialised at startup and can be hot-reloaded
via `/forge-reload` without restarting the server.

### Process execution

The server can spawn and manage interactive child processes (e.g. `gemini` CLI) via the `/exec` family
of endpoints. Each running process is tracked as a **session** identified by a `sessionId`.
stdout and stderr are streamed to the browser via Server-Sent Events (SSE).
See [Session model](#session-model) for the full lifecycle.

## Startup

### Command

```
node tools/local-server.js <root1> [<root2> ...] [--port <port>]
```

- `<root1>`, `<root2>`, ... — absolute paths to allowed roots (at least one required)
- `--port` — optional, defaults to `3000`

### Batch file

`tools/start-server.bat` is the standard Windows launcher. It defines the allowed roots
for the current machine and starts the server.

Edit `start-server.bat` to add or remove roots for your environment:

```bat
@echo off
node "%~dp0local-server.js" ^
  "C:\Users\Remi\Dropbox" ^
  "C:\Users\Remi\GoogleDrive" ^
  "C:\Users\Remi\OneDrive" ^
  --port 3000
pause
```

Place a shortcut to `start-server.bat` in the Windows Startup folder to launch it at boot.

### Confirmation output

```
Local server started on http://localhost:3000
Allowed roots:
  C:\Users\Remi\Dropbox
  C:\Users\Remi\GoogleDrive
  C:\Users\Remi\OneDrive
Ctrl+C to stop.
```

### Port conflict

If port 3000 is already in use, the server exits with:
```
Error: port 3000 is already in use. A server may already be running.
```

## API Contract

All paths in `path=` and `dir=` parameters are **absolute paths** on the local filesystem.
The server validates each path against the allowed roots before acting.

CORS headers are set on every response (`Access-Control-Allow-Origin: *`) so that pages
served from `http://localhost:3000` can call the API endpoints without restriction.

---

### GET /ping

Server availability check. Used by HTML pages on load.

**Request:**
```
GET http://localhost:3000/ping
```

**Response:**
- `200` + `{ "ok": true }`

---

### GET /file

Read a file from disk.

**Request:**
```
GET http://localhost:3000/file?path=C:/Users/Remi/Dropbox/AfrSCM/historique/data-2026-05-27.json
```

**Responses:**
- `200` + file content with appropriate `Content-Type`
- `400` + `{ "error": "Missing parameter: path" }` — parameter absent
- `403` + `{ "error": "Access denied" }` — path outside allowed roots
- `404` + `{ "error": "File not found" }` — file does not exist

---

### POST /file

Write or overwrite a file on disk. Creates intermediate directories if needed.
Body is written as-is — no validation by the server.

**Request:**
```
POST http://localhost:3000/file?path=C:/Users/Remi/Dropbox/AfrSCM/historique/changes-2026-05-27.json
Content-Type: application/json

{ ...content... }
```

**Responses:**
- `200` + `{ "ok": true }` — write succeeded
- `400` + `{ "error": "Missing parameter: path" }` — parameter absent
- `403` + `{ "error": "Access denied" }` — path outside allowed roots
- `500` + `{ "error": "..." }` — write failed (permissions, disk full, etc.)

---

### DELETE /file

Delete a file from disk. Idempotent — returns `200` even if the file did not exist.

**Request:**
```
DELETE http://localhost:3000/file?path=C:/Users/Remi/Dropbox/AfrSCM/historique/changes-2026-05-27.json
```

**Responses:**
- `200` + `{ "ok": true }`
- `400` + `{ "error": "Missing parameter: path" }` — parameter absent
- `403` + `{ "error": "Access denied" }` — path outside allowed roots
- `500` + `{ "error": "..." }` — deletion failed

---

### GET /dir

List the contents of a directory.

**Request:**
```
GET http://localhost:3000/dir?path=C:/Users/Remi/Dropbox/AfrSCM/historique
```

**Response body:**
```json
{
  "entries": [
    { "name": "data-2026-05-27.json", "type": "file" },
    { "name": "data-2026-06-01.json", "type": "file" },
    { "name": "changes-2026-06-01.json", "type": "file" },
    { "name": "subdir", "type": "dir" }
  ]
}
```

Entries are sorted alphabetically by name. Subdirectories are included with `"type": "dir"`.

**Responses:**
- `200` + `{ "entries": [...] }`
- `200` + `{ "entries": [] }` — directory exists but is empty
- `400` + `{ "error": "Missing parameter: path" }` — parameter absent
- `403` + `{ "error": "Access denied" }` — path outside allowed roots
- `404` + `{ "error": "Directory not found" }` — path does not exist

---

### POST /forge

Proxy a tool call to the Forge v2 MCP server. The Forge instance must be initialised
(automatic at startup). Returns the tool result directly.

**Request:**
```
POST http://localhost:3000/forge
Content-Type: application/json

{ "tool": "tool_name", "args": { ...tool arguments... } }
```

**Responses:**
- `200` + tool result (JSON)
- `400` + `{ "error": "Missing field: tool" }` — tool name absent
- `400` + `{ "error": "Invalid JSON body" }` — body not parseable
- `503` + `{ "error": "Forge proxy not available" }` — Forge failed to initialise
- `500` + `{ "error": "..." }` — tool execution error

---

### POST /forge-reload

Hot-reload the Forge tool and format registries without restarting the server.
Picks up changes to `forge-tools.json` and `forge-formats.json`.
Handler JS modules are not reloaded (ESM cache) — only JSON config changes are applied.

**Request:**
```
POST http://localhost:3000/forge-reload
```

**Responses:**
- `200` + `{ "ok": true, "roots": [...], "tools": [...] }` — reload succeeded
- `500` + `{ "error": "..." }` — reload failed

---

### POST /exec

Start a new process session or send input to an existing one.

**Start a session** (no `sessionId` in body):
```
POST http://localhost:3000/exec
Content-Type: application/json

{ "cwd": "C:/Users/Remi/Development/my-project", "command": "gemini" }
```

- `cwd` — working directory for the process (must be within an allowed root)
- `command` — executable to launch (default: `"gemini"`)

**Response:**
- `200` + `{ "sessionId": "abc123" }` — session created, process started
- `400` + `{ "error": "Missing field: cwd" }` — cwd absent
- `403` + `{ "error": "Access denied" }` — cwd outside allowed roots
- `500` + `{ "error": "..." }` — process failed to start

**Send input to an existing session:**
```
POST http://localhost:3000/exec
Content-Type: application/json

{ "sessionId": "abc123", "input": "bonjour" }
```

- `sessionId` — identifier returned when the session was created
- `input` — text to send to the process stdin (newline appended automatically)

**Responses:**
- `200` + `{ "ok": true }`
- `404` + `{ "error": "Session not found" }` — sessionId unknown or expired
- `500` + `{ "error": "..." }` — write to stdin failed

---

### GET /exec/stream

Stream stdout and stderr of a running session via Server-Sent Events (SSE).
The browser connects once and receives events as the process produces output.

**Request:**
```
GET http://localhost:3000/exec/stream?sessionId=abc123
```

**Event format:**

Each SSE event has a `type` field and a `data` field:

| Event type | Meaning |
|------------|---------|
| `stdout` | A line of standard output from the process |
| `stderr` | A line of standard error from the process |
| `exit` | Process has exited — `data` contains the exit code |
| `error` | Stream error — `data` contains the error message |

Example event stream:
```
event: stdout
data: Welcome to Gemini CLI

event: stdout
data: > 

event: exit
data: 0
```

**Responses:**
- `200` + SSE stream (`Content-Type: text/event-stream`)
- `404` + `{ "error": "Session not found" }` — sessionId unknown or expired

---

### DELETE /exec

Terminate a running session and kill the associated process.

**Request:**
```
DELETE http://localhost:3000/exec?sessionId=abc123
```

**Responses:**
- `200` + `{ "ok": true }` — process terminated
- `404` + `{ "error": "Session not found" }` — sessionId unknown or already terminated

---

### GET /*

Serve a static file by its absolute path encoded in the URL.

**Request:**
```
GET http://localhost:3000/C:/Users/Remi/Dropbox/AfrSCM/AssistantIA/index.html
```

The server strips the leading `/`, reconstructs the absolute path, validates against
allowed roots, and serves the file with the appropriate `Content-Type`.

**Supported types:** `.html`, `.js`, `.json`, `.css`, `.png`, `.ico`

**Responses:**
- `200` + file content
- `403` + `{ "error": "Access denied" }` — path outside allowed roots
- `404` + `{ "error": "File not found" }` — file does not exist

---

### OPTIONS (preflight CORS)

**Response:** `204` with CORS headers. Handles browser preflight requests transparently.

## Session model

### What a session is

A **session** represents a single running child process (e.g. `gemini`). It is identified
by a `sessionId` — a random string generated at creation. Sessions are held in memory only;
they do not survive a server restart.

### Session lifecycle

```
POST /exec (no sessionId)
  → process spawned in cwd
  → sessionId returned
  → GET /exec/stream opened by browser (SSE)
  → POST /exec (with sessionId + input) to send prompts
  → DELETE /exec or process exits naturally → session removed
```

### Inactivity timeout

Sessions with no stdin input for **30 minutes** are automatically terminated and removed.
The SSE stream receives an `exit` event before the connection closes.

### Session map

Sessions are stored in a `Map` in server memory:

```
sessionId → { process, cwd, command, lastActivity, sseClients[] }
```

- `process` — the Node.js `ChildProcess` instance
- `cwd` — working directory the process was started in
- `command` — the command that was launched
- `lastActivity` — timestamp of the last stdin write (used for timeout)
- `sseClients` — list of active SSE response objects for this session

Multiple SSE connections to the same session receive the same output stream —
useful if the dashboard is open in several tabs.

## How projects use the server

### Bootstrap file

Each project has a small bootstrap HTML file (e.g. `Comite RSE.html`) that:
1. Checks server availability via `GET /ping`
2. If available: redirects to `http://localhost:3000/<absolute-path-to-project>/index.html`
3. If unavailable: displays "Start the local server before opening this file"

The bootstrap file is the **only file opened directly via `file://`**.
All project pages are then served via `http://localhost:3000/...` and can use `fetch()` freely.

### Building API paths in pages

Pages construct absolute paths for `/file` and `/dir` calls from their own URL:

```javascript
// Extract absolute path base from current page URL
// http://localhost:3000/C:/Users/Remi/Dropbox/AfrSCM/reunions/index.html
// -> base = "C:/Users/Remi/Dropbox/AfrSCM/reunions/"
const base = window.location.pathname.replace(/^\//, '').replace(/[^/]+$/, '');

// Build an API call
fetch(`/file?path=${base}historique/data-2026-05-27.json`);
```

Relative `fetch()` calls for static assets (scripts, CSS, other HTML pages) work
without any special handling — the browser resolves them automatically.

### Using exec sessions

A typical terminal widget in an HTML page follows this pattern:

```javascript
// 1. Start a session
const { sessionId } = await fetch('/exec', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ cwd: projectPath, command: 'gemini' })
}).then(r => r.json());

// 2. Open SSE stream
const es = new EventSource(`/exec/stream?sessionId=${sessionId}`);
es.addEventListener('stdout', e => appendToTerminal(e.data));
es.addEventListener('stderr', e => appendToTerminal(e.data, 'error'));
es.addEventListener('exit',   e => markSessionEnded(e.data));

// 3. Send input
await fetch('/exec', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ sessionId, input: userText })
});

// 4. Terminate
await fetch(`/exec?sessionId=${sessionId}`, { method: 'DELETE' });
```

### Availability indicator

Each project HTML page should display a discrete status indicator reflecting server state:

| State | Display | Meaning |
|-------|---------|---------|
| Connected | green dot | Server available, saves active |
| Disconnected | orange dot | Server absent — changes not saved |
| Error | red dot | Server reachable but write failed |

## Security

### Path traversal prevention

Every path (from `path=`, `dir=`, `cwd=`, or the static file URL) is resolved to an absolute
path and checked against the allowed roots list before any filesystem operation or process spawn.
A path that does not start with one of the allowed roots returns `403` immediately.

### Process execution scope

`POST /exec` validates `cwd` against allowed roots before spawning any process.
Only directories within allowed roots can be used as working directories.
The command itself is not restricted — only the working directory is validated.

### Local only

The server binds to `127.0.0.1` only — not accessible from other machines on the network.

### No authentication

The server has no authentication mechanism. It is intended for single-user local
development only. Do not expose it to a network.

## What the server does not do

- No JSON validation — body is written as-is
- No business logic — no knowledge of artifacts, revisions, or schemas
- No file filtering — `/dir` returns all entries; filtering by name pattern is the caller's responsibility
- No authentication
- No automatic startup — must be launched manually (or via Startup folder shortcut)
- No session persistence — exec sessions are lost on server restart
- No command allowlist — any executable can be launched via `/exec` from within an allowed root
