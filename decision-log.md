# Decision log

Newest first. One line per decision, with the reason.

| Date | Decision | Why |
|---|---|---|
| 2026-11 | One repo per peripheral, plus briss-chip, briss-game, briss-docs | Each sub-group owns its repo; tests stay fast |
| 2026-11 | `briss_regs.h` is written by hand by AND's Drivers sub-group, copied from each `SPEC.md` | A generator would be hard for beginners to maintain |
| 2026-11 | Target Tiny Tapeout GF180 shuttle TTGF27a with TinyQV | Open CPU with a ready peripheral interface; Tiny Tapeout is sponsoring two tiles |
| 2026-11 | Whoever writes the Verilog doesn't write its test | Catches misread specs |
