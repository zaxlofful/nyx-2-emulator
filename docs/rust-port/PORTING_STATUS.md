# Rust Porting Status

Phase 0 inventory and proposal are complete. No Rust code has been written.
The source mismatch and questions in `OPEN_QUESTIONS.md` require review before
Phase 1.

## Python module checklist

| Python module | Status |
|---|---|
| `main.py` | not started |

There are no other Python modules in the checkout. The standalone `main.rs` is
not part of this Python-module checklist; its scope is an open question.

## Next up

1. Review `INVENTORY.md`, `ARCHITECTURE.md`, and `DEPENDENCIES.md`.
2. Resolve the scope and UI-toolkit questions in `OPEN_QUESTIONS.md`.
3. After approval, begin Phase 1 for the agreed repository scope.

## Parity and tests

No tests or parity harness are present. Nothing is ported or parity-verified.

## Performance hotspots

None identified in the Python reference; it contains no emulator execution
core.
