# Python Source Inventory

## Repository scope found

The Python source in this checkout is a single PyQt5 game-library GUI, not a
Nintendo Switch emulator implementation. It lists game covers, copies selected
files, and shows game/OS labels. There are no Python CPU, memory, loader,
kernel, IPC, filesystem/crypto, GPU, audio, or input modules to port.

The repository also contains a standalone `main.rs` with a five-instruction
toy CPU. It is not imported by the Python program and is not evidence of
Python emulator behavior; its status and intended relationship to this task
need review before porting it.

## Python modules

| Path | Lines | Purpose | Subsystem | Internal dependencies | External dependencies |
|---|---:|---|---|---|---|
| `main.py` | 201 | PyQt5 desktop UI for listing games, selecting a game, adding a cover image, and copying/displaying an OS file label. | Frontend/UI | None | `PyQt5.QtWidgets`, `PyQt5.QtGui`, `PyQt5.QtCore` |

### Dynamic patterns and state

| Pattern | Location | Porting consideration |
|---|---|---|
| Assigns a per-widget `mousePressEvent` lambda that closes over each game (`g=game`). | `main.py:128`, `main.py:177` | Replace with an explicit click callback carrying the selected game; preserve per-item capture semantics. |
| Qt signals connect actions to bound methods. | `main.py:56`, `main.py:60`, `main.py:64`, `main.py:69` | Use the target GUI toolkit's event/action callbacks. |
| `os_installed` is assigned only after the user completes OS installation, but is read without initialization. | `main.py:86`, `main.py:96` | Accessing OS Settings before installation can raise `AttributeError`; preserve and document during a behavior-preserving port rather than silently fixing it. |
| Relative paths and process working directory determine game data and cover paths. | `main.py:15-16`, `main.py:79-84`, `main.py:159-163`, `main.py:180-194` | Preserve these exact relative-path semantics unless a separately approved change says otherwise. |

No uses of `getattr`/`setattr` dispatch, monkey-patching beyond the widget event
assignment above, metaclasses, `eval`/`exec`, arbitrary-precision arithmetic,
threads, async code, or GIL-dependent synchronization were found. There is no
module-level mutable state beyond path constants; the window owns its mutable
game list and installed-OS field.

## Existing tests

No Python test files, test directories, test configuration, or Rust Cargo
manifest were found in this checkout. `games.json` is the application's
runtime data file, not a test fixture.

## Observed behavior boundaries

- `install_os` copies the selected file into `assets/covers/` and records its
  title/path only in memory; it does not parse or install an operating system.
- `add_game` adds a title and copied image path to `games.json`; there is no
  game executable loading or launching.
- `toggle_os` only displays “No OS Found” and has no visible caller.
- `dark_theme` is initialized but otherwise unused.
- Invalid or empty `games.json` content produces an empty game list.
