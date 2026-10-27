# Python-to-Rust Dependency Mapping

The only third-party Python package imported in this repository is PyQt5.
Proposed Rust choices below are candidates for the UI-only source actually
present; confirm the GUI toolkit choice before Phase 1.

| Python package / module | Rust mapping | Justification |
|---|---|---|
| `PyQt5.QtWidgets`, `PyQt5.QtGui`, `PyQt5.QtCore` | `eframe` + `egui`, with `rfd` for native file dialogs and `image` for image decoding/scaling | Provides a Rust-native desktop UI, event handling, image display, and file dialogs without requiring a Python runtime. This is a toolkit migration, so exact widget appearance and dialog behavior require parity review. |
| `json` (Python standard library) | `serde` + `serde_json` | Typed serialization/deserialization for the game list and JSON formatting equivalent to the persisted catalog. |
| `os` (Python standard library) | `std::fs` + `std::path` | File existence, directory creation, basename handling, and byte-for-byte copying are available in the Rust standard library. |
| `sys` (Python standard library) | `std::env` and the selected GUI application's entry point | Reads process arguments and starts/exits the application. |

No Python crypto, compression, binary-parsing, numerical-array, audio,
graphics-backend, or emulator-specific package is imported. Do not add
dependencies for absent functionality during this phase.
