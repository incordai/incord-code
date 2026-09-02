# incord-code
An autonomous coding harness. It plans work, delegates it to a coding agent you already have installed and logged in, and proves the work was done before it calls it done.
# Incord Code

`incode` — an autonomous coding harness. It plans work, delegates it to a coding
agent you already have installed and logged in, and proves the work was done
before it calls it done.

It is **not** another coding assistant. The agent you install brings the loop,
the tools and the model. What this adds is the part an agent cannot do for
itself: refuse to start before the project is specified, keep the files that
judge the work out of the agent's reach, and close a task only on a command that
exited zero.

---

## What it does

- **Delegates, on your own subscription.** The work goes to an installed agent
  CLI that authenticates itself. This harness reads, holds and transmits no
  credential of its own.
- **Refuses to code an unspecified project.** About, Goals, Synopsis, PRD and a
  roadmap must exist before a source file may be written. On an existing
  codebase it reads the code first and asks only what the code cannot answer.
- **Proves completion.** A task reaches `Done` only through a command that
  exited zero. No model's report — however confident, from whichever model —
  reaches that state.
- **Protects the exam.** Tests, CI config, migrations, `.env` files and the
  ledger are refused as write targets. An agent that can edit its own tests will
  eventually make a failing one pass by editing it.
- **Routes by role.** Reading and renaming go to a cheap model; judgement goes to
  an expensive one. You decide which, per role.
- **Bounds the spend.** A token ceiling stops the next request. What was spent is
  always counted in full, including the request that crossed the line.
- **Undoes a turn.** The tree is snapshotted before a delegated turn; `/undo`
  restores only what actually changed.
- **Remembers across sessions.** Context, standing rules, skills, already-solved
  work and audit findings are read from the shared memory service — not restated
  by you every morning.

## Requirements

- **Rust 1.88+** to build. This is measured, not guessed — it is the highest
  `rust-version` declared by any locked dependency's own manifest (`home
  0.5.12`; 154 of the 209 declare one at all), and it is
  declared in `Cargo.toml` so cargo refuses an older toolchain by name rather
  than failing deep inside a dependency.
- **A coding agent CLI**, installed and logged in. `/doctor` names what it found
  and what is missing.
- **Optional:** the Incord memory service on `http://127.0.0.1:9180`. Every part
  that uses it is a plugin and defaults to on; without it the harness still
  works and simply knows less. Point elsewhere with `INCORD_MEMORY_URL`.

## Install

```sh
git clone <this repo> incord-code
cd incord-code
cargo build --release
```

The binary lands at `target/release/incode`. Put it on your `PATH`:

```sh
# Linux / macOS
install -m755 target/release/incode ~/.local/bin/incode

# Windows (PowerShell)
Copy-Item target\release\incode.exe $env:USERPROFILE\.local\bin\
```

Then check the machine is ready:

```sh
incode          # start a session, then type /doctor
```

## Usage

```
incode [OPTIONS]

  -m, --model <NAME>        Model to use (or set INCORD_MODEL)
  -w, --workspace <PATH>    Workspace root (default: current directory)
  -p, --permission-mode <M> read-only | workspace-write | danger-full-access | prompt | allow
      --no-env-filter       Pass the full environment to spawned commands
      --admin               Print the operator's settings and where each came from
  -t, --task <TEXT>         Run one instruction and exit (needs no API credential)
  -r, --resume <ID>         Continue an agent session by its id
      --add-dir <PATH>      Also work in this directory (repeatable)
  -h, --help                Print help
  -V, --version             Print version
```

One instruction, no session:

```sh
incode --task "add a health endpoint and a test for it"
```

### Commands inside a session

| Command | What it does |
|---|---|
| `/help` | Every command, with what it does |
| `/doctor` | Whether this session can work: agents, login, guards, specs, ledger, memory |
| `/status` | Model, workspace, permission tier, tokens spent, and the ledger's progress |
| `/admin` | Which model, ceiling and check this session resolved to, and which layer decided each |
| `/run` | Work the roadmap ledger until it is empty or something needs a person |
| `/guards` | The paths the agent is not allowed to write |
| `/links <file>` | What else uses what this file defines, and what it depends on |
| `/undo` | Put the files from the last change back as they were |
| `/plan` · `/act` | Propose changes without making them · apply them, within the guards |
| `/model [name]` | Show or switch the model |
| `/clear` · `/save` · `/sessions` · `/resume <id>` | Session handling |
| `/remember <text>` · `/memories <query>` · `/forget <ref>` | The shared long-term memory |
| `/skills` | The skills this session loaded from the shared store |
| `/bg <cmd>` · `/jobs` · `/kill <id>` | Background shell jobs |
| `/worktree [cmd]` | Git worktree create / list / remove |
| `/tools` · `/mcp` · `/env` · `/compact` | Where tools, MCP servers and context are handled |
| `/quit` | Exit |

## Configuration

Settings resolve in this order — **flag > environment > admin file > built-in** —
and `--admin` prints which layer decided each value.

`~/.incord-code/admin.json`:

```json
{
  "roles": {
    "explore":   ["haiku", "sonnet"],
    "grunt":     ["haiku", "sonnet"],
    "diagnose":  ["sonnet", "opus"],
    "implement": ["sonnet", "opus"],
    "frontend":  ["sonnet", "opus"],
    "plan":      ["opus", "sonnet"],
    "review":    ["opus", "sonnet"]
  },
  "budget_tokens": 2000000,
  "verify_command": ["cargo", "test"]
}
```

A role's candidates are an **ordered fallback chain**. A candidate may name the
agent that should serve it as `agent:model`; a bare model name means "whichever
agent is running". A candidate naming an agent that is not installed is skipped
rather than treated as an error, so one table works on every machine.

Useful environment variables:

| Variable | What it sets |
|---|---|
| `INCORD_MODEL` | The session's model |
| `INCORD_ROLE_<ROLE>` | That role's ordered candidates, comma-separated |
| `INCORD_BUDGET_TOKENS` | The token ceiling for a run |
| `INCORD_MEMORY_URL` | Where the memory service listens |
| `INCORD_CODE_DIR` | Where this harness keeps its settings |
| `INCORD_TRACE` | Say what the harness did, as it happens |
| `INCORD_SEARCH_URL` / `INCORD_SEARCH_KEY` | The search endpoint for the API path |

### What lives where

| Path | What it holds |
|---|---|
| `~/.incord-code/admin.json` | Your settings — models, ceiling, check, plugins |
| `~/.incord-code/sessions/sessions.json` | Session pointers, so a conversation can be reopened |
| `~/.incord-code/history.txt` | The lines you typed |
| `<project>/.incord/ledger.json` | What is being worked on in *this* checkout |
| `<project>/.incord/contracts.json` | Interfaces frozen before the work was split up |
| `<project>/.incord/guards.json` | Extra protected paths for this project |

Per-project state lives **in the project**; `~/.incord-code` is yours, not the
project's.

## Security notes

- **No credential is read or held.** The agent authenticates itself; this
  harness only detects that a binary exists.
- **Spawned commands get a bounded environment**, not a safe one. The allowlist
  deliberately passes cloud, git-forge and model credentials through, because
  `gh` and a deploy cannot work without them. Unset what you do not want
  reachable before starting a session. `--no-env-filter` widens this, not
  narrows it.
- **`history.txt` is plain text.** Anything you type is written there, a
  credential included. Delete the file if you paste one.
- **Web reads refuse the private network** by name and again by resolved
  address, with the connection pinned to the address that was checked. A
  redirect to a different host is checked by name only.
- **Retrieved text is data, never instructions.** What comes back from memory,
  the web or a tool is labelled as such before the model sees it.

## Development

```sh
cargo test                          # 780+ tests
cargo clippy --all-targets -- -D warnings
cargo fmt
```

The documents under `docs/` are the input the loop reads, not decoration:
`PRD.md` (what and why), `ARCHITECTURE.md` (the shape), `CONVENTIONS.md` (how
the code should read), `ASSUMPTIONS.md` (what was decided without asking),
`RUNNER.md` (the autonomous loop), `VERIFICATION.md` (what counts as done).

## Licence

MIT — see [LICENSE](LICENSE).

