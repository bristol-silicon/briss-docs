# System spec

> Owned by **OR**. Sections marked **TBD** are filled in before the December spec freeze.

## The chip

- CPU: TinyQV (RISC-V RV32EC) from [MichaelBell/ttgf0p2-tinyQV](https://github.com/MichaelBell/ttgf0p2-tinyQV)
- Process: GlobalFoundries GF180 via Tiny Tapeout (TTGF27a)
- Size: 3×4 tiles total, shared by the CPU and all peripherals
- Clock: 24 MHz (one clock for everything)
- Program and data live in external QSPI flash/PSRAM

## How a peripheral connects

Every peripheral has the same ports (see `src/peripheral.v` in the template):

| Signal | Width | Meaning |
|---|---|---|
| `clk`, `rst_n` | 1 | Clock, active-low reset |
| `ui_in` | 8 | Chip input pins (shared by all peripherals) |
| `uo_out` | 8 | This peripheral's output pins (OR picks which peripheral drives each pin) |
| `address` | 6 | Register address (byte offset) |
| `data_in` | 32 | Data from the CPU |
| `data_write_n` | 2 | `11` = no write, `00` = byte, `01` = half-word, `10` = word |
| `data_read_n` | 2 | Same encoding for reads |
| `data_out` | 32 | Data to the CPU |
| `data_ready` | 1 | Set high when a read result is ready |
| `user_interrupt` | 1 | Interrupt to the CPU |

## Slot map — TBD

| Slot | Peripheral | Type | Size budget (cells) |
|---|---|---|---|
| | periph-video | full | TBD |
| | periph-audio | full | TBD |
| | periph-gamepad | byte | TBD |
| | periph-rotary | byte | TBD |
| | periph-score | TBD | TBD |
| | periph-random | byte | TBD |
| | periph-collision | TBD | TBD |
| | periph-maths | full | TBD |

## Pin plan — TBD

| Pin | Direction | Used by |
|---|---|---|
| `ui_in[0..7]` | in | TBD (gamepad, rotary) |
| `uo_out[0..7]` | out | TBD (VGA, audio) |

## Backups

If a peripheral misses the design freeze, OR swaps in an existing, already-tested TinyQV peripheral in its slot so the chip still works.
