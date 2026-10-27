# Proposed Rust Architecture

## Scope and workspace

The checked-in Python reference currently implements a small desktop UI, not
the emulator subsystems named in the port request. Accordingly, the initial
workspace proposal is deliberately limited to the behavior actually present:

```text
crates/
  frontend/       # Application entry point, UI, and app-owned state
xtask/             # Optional development/parity commands, after scope approval
```

`main.py` maps to the `frontend` crate:

- `GameItem` -> a game-cover presentation component.
- `MainWindow` -> application/window state and UI actions.
- `load_games` / `save_games` -> game-catalog persistence.
- `install_os` -> the current file-copy and in-memory label behavior; this is
  not an operating-system installer.

There are no Python modules to map to `common`, `memory`, `cpu`, `loader`,
`kernel`, `services`, `fs`, `gpu`, `audio`, or `input` crates. Do not create
empty emulator crates or treat the unrelated standalone `main.rs` toy CPU as
an implementation of Python behavior. Revisit the workspace only if the
repository scope is clarified or additional reference source is supplied.

## Dependency order

For the source currently present:

1. Establish the UI toolkit and application entry point.
2. Port game-catalog JSON loading/saving and its empty/invalid-file behavior.
3. Port the game list, image scaling, selection details, and add-game flow.
4. Port the OS-file copy/settings dialogs and current uninitialized-state
   behavior.
5. Add behavior-focused tests and a parity harness for the Python behaviors
   that can be exercised without proprietary files.

No CPU-to-memory or emulator dependency graph can be derived from this
repository. In particular, do not add commercial game files, firmware, BIOS
images, or console keys to test the current UI.

## State ownership and sharing

Use one application state owned by the UI event loop. It should contain the
game records, selected game, and the optional OS-install record, alongside
presentation state as required by the chosen toolkit. UI callbacks update this
state and request a redraw; persistence remains an explicit operation matching
the Python callbacks.

The Python source has no guest memory, kernel object table, scheduler, or GPU
command stream. Those ownership questions are therefore not applicable to the
current code and should not be answered with speculative shared state.

## Threading

Python creates a Qt application and runs its event loop on the main thread.
There are no explicit worker threads, asynchronous tasks, or GIL-dependent
shared data. Keep UI and application state single-owner on the Rust UI thread.
Do not introduce `Arc`, `Mutex`, or atomics without a concrete concurrent
operation; file copy and JSON operations currently run synchronously in UI
callbacks.

## Review boundary

Before Phase 1, confirm whether the intended port is (a) a behavior-preserving
rewrite of the existing PyQt5 GUI, or (b) an emulator port whose Python
implementation is absent from this checkout. Also clarify whether `main.rs`
is an intentional part of the target. No Rust implementation should begin
until this mismatch is reviewed.
