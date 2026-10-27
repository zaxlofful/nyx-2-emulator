# Open Questions

1. **Port scope:** The only Python module is `main.py`, a PyQt5 catalog UI;
   this checkout contains no Switch-emulation implementation. Is the requested
   target the existing UI, or is emulator source missing from the repository?
   Reference: `main.py:18-200`.
2. **Existing Rust file:** Should the unrelated `main.rs` five-op toy CPU be
   included in the eventual port, retained independently, or excluded?
   Reference: `main.rs:5-81`.
3. **UI toolkit:** Is an `eframe`/`egui` frontend acceptable, or must the Rust
   frontend preserve Qt specifically? The repository has no existing Rust
   manifest or UI toolkit choice.

Phase 0 is complete pending answers/review. Do not begin Rust implementation
until the scope mismatch is resolved.
