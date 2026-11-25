# Peripheral spec: Timer (worked example)

> **This is an example.** The timer is made up and isn't on our chip. It shows what a finished `SPEC.md` looks like, so copy its style, not its contents.
> See [writing-your-spec.md](writing-your-spec.md) for how its answers turned into these registers.

| | |
|---|---|
| **Team / sub-group** | Example / Timer |
| **Owners** (2 names) | Alex Example, Sam Sample |
| **Type** | Full peripheral |
| **Slot number** | Given by OR |
| **Size budget** | Given by OR, in cells |
| **Status** | Example |

## 1. Purpose

Counts down from a number the game chooses and raises a flag when it reaches zero. The game uses it to do things at a steady pace (e.g. move the enemies every half second) without wasting time in counting loops.

## 2. Pins

No pins. The timer only talks to the CPU.

| Pin | Direction | Use |
|---|---|---|
| none | | |

## 3. Registers

| Offset | Name | R/W | Bits | Reset value | Meaning |
|---|---|---|---|---|---|
| 0x00 | CTRL | R/W | 1:0 | 0 | Bit 0 `ENABLE`: 1 = counting, 0 = paused. Bit 1 `REPEAT`: 1 = start again from `RELOAD` after reaching zero |
| 0x04 | RELOAD | R/W | 23:0 | 0 | The number to count down from, in clock ticks (24 MHz, so 12,000,000 = 0.5 s). Writing it also loads `COUNT` |
| 0x08 | COUNT | R | 23:0 | 0 | The current count, in clock ticks. Writing does nothing |
| 0x0C | STATUS | R/W | 0 | 0 | Bit 0 `DONE`: set to 1 when `COUNT` reaches zero. **Write 1 to clear**; writing 0 does nothing |

Bits not listed read as 0. Offsets 0x10–0x3C are unused and read as 0.

## 4. Behaviour

- While `ENABLE` = 1 and `COUNT` > 0, `COUNT` goes down by 1 every clock tick.
- When `COUNT` goes from 1 to 0, `DONE` is set to 1.
  - If `REPEAT` = 1, `COUNT` is loaded from `RELOAD` on the same tick, so the timer keeps going with no gap.
  - If `REPEAT` = 0, `COUNT` stays at 0 until the game writes `RELOAD` again.
- Writing `RELOAD` immediately sets `COUNT` to the new value. It does not clear `DONE`.
- `ENABLE` = 0 pauses the count. Setting it back to 1 carries on from where it stopped.
- If `DONE` is being set on the same tick the game writes 1 to clear it, **setting wins**, so no event is lost.
- `user_interrupt` is not used (always 0). The game checks `STATUS` instead.

## 5. Checks

- **T1:** after reset, `CTRL`, `RELOAD`, `COUNT` and `STATUS` all read 0.
- **T2:** writing `CTRL` = 0x3 and `RELOAD` = 0x123456 reads back the same values. Writing `RELOAD` = 0xFFFFFFFF reads back 0x00FFFFFF.
- **T3:** writing `RELOAD` = 1000 makes `COUNT` read 1000 straight away (with `ENABLE` = 0).
- **T4:** with `RELOAD` = 1000 and `ENABLE` = 1, after 100 clock ticks `COUNT` is about 900 (allow ±5 for bus delays).
- **T5:** with `RELOAD` = 50, `ENABLE` = 1 and `REPEAT` = 0, after 60 ticks `COUNT` = 0 and `DONE` = 1.
- **T6:** with `RELOAD` = 50, `ENABLE` = 1 and `REPEAT` = 1, after 60 ticks `DONE` = 1 and `COUNT` is between 35 and 45 (it reloaded and kept counting).
- **T7:** with `DONE` = 1, writing 0 to `STATUS` leaves it at 1, and writing 1 clears it to 0.
- **T8:** with the timer counting, setting `ENABLE` = 0 stops `COUNT` changing for 100 ticks.

## 6. How the game uses it

```c
// Move the enemies every half second.
BRISS_TIMER->reload = 12000000;                   // 0.5 s at 24 MHz
BRISS_TIMER->ctrl   = TIMER_ENABLE | TIMER_REPEAT;

while (1) {
    if (BRISS_TIMER->status & TIMER_DONE) {
        BRISS_TIMER->status = TIMER_DONE;         // "got it": clear the flag
        move_enemies();
    }
    read_buttons_and_move_player();
}
```

## 7. Versions

| Version | Done when | Target |
|---|---|---|
| v0.1 | All 4 registers exist and read back (no counting yet); T1, T2 pass | Before Christmas |
| v0.2 | Counting and `DONE` work; T3–T5, T7 pass | Mid February |
| v1.0 | `REPEAT` and pause work; T1–T8 pass; under size budget | Mid March (design freeze) |

## 8. Open questions

- 24 bits only reaches about 0.7 s (2²⁴ ÷ 24 MHz). Is that long enough? If not, do we add a "divide the clock by 256 first" option rather than making the counter bigger, since a bigger counter costs area?
- Would the game ever want two timers at once?
