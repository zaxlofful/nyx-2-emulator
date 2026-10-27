# Bugs Found During Inventory

This is an inventory finding, not a request to change the Python reference.
If the UI is ported, preserve the behavior and mark the corresponding Rust
code with the required `PORT-NOTE` until a separate bug-fix phase is approved.

| Reference | Finding | Porting treatment |
|---|---|---|
| `main.py:86`, `main.py:96` | `self.os_installed` is created only after a successful OS-file selection and title entry. Opening OS Settings before that can raise `AttributeError`. | Preserve the observable behavior in a parity port; do not initialize it as `None` unless a later fix is explicitly approved. |
