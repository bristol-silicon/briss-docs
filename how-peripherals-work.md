# How peripherals work

How the CPU and your peripheral talk to each other, explained without assuming you know anything about hardware.

## The post-box analogy

Imagine your peripheral is a wall of **numbered post boxes**.

```
   address:   0x00      0x04      0x08      0x0C
            ┌───────┬─────────┬─────────┬─────────┐
            │ X_POS │  Y_POS  │ COLOUR  │ STATUS  │   ← registers
            └───────┴─────────┴─────────┴─────────┘
```

- When the CPU wants to tell your peripheral something, it **posts a number into a box**: "put 120 in box 0x00". That's a **write**.
- When the CPU wants to know something, it **looks in a box**: "what's in box 0x0C?". That's a **read**.
- Each box is a **register**: a little piece of memory inside your peripheral. Its **address** is the box number.

Your Verilog decides:
- **what happens when a number is posted** (e.g. store it, start a sound, clear a flag), and
- **what number is in each box when the CPU looks** (e.g. the stored value, or a button state).

### Example: a gamepad peripheral

| Address | Name | The CPU... | Meaning |
|---|---|---|---|
| 0x00 | BUTTONS | reads | Bit 0 = left, bit 1 = right, bit 2 = fire. 1 means pressed |

In C, the game does:

```c
if (GAMEPAD_BUTTONS & 0x1) player_x -= 1;   // left pressed → move left
```

The CPU reads box 0x00. Your Verilog answers with the button states. The game moves the player. That's the whole loop.

## The wires between the CPU and your peripheral

Every BriSS peripheral has exactly the same wires (the template already has them):

| Wire | Direction | What it carries |
|---|---|---|
| `clk` | in | The clock: a tick that keeps everything in step (24 million ticks a second) |
| `rst_n` | in | Reset. When it's 0, set everything back to its starting value |
| `address` | in | **Which box** the CPU is talking to (0x00–0x3F) |
| `data_in` | in | **The number** the CPU is posting (on a write) |
| `data_write_n` | in | **Is the CPU writing?** `11` = no. Anything else = yes (and how many bytes) |
| `data_read_n` | in | **Is the CPU reading?** Same encoding |
| `data_out` | out | **Your answer**: the number in the box the CPU is reading |
| `data_ready` | out | "My answer is ready". Usually just `1` |
| `ui_in` | in | The chip's input pins (buttons, knob...) |
| `uo_out` | out | The chip's output pins (VGA, sound...) |
| `user_interrupt` | out | "Hey CPU, something happened!" (optional) |

### Why `_n`?
`_n` means "active low": the signal does its job when it's **0**, not 1. So `rst_n = 0` means "reset now", and `data_write_n = 11` means "not writing".

### Byte, half-word, word
The CPU can write 1 byte (8 bits), 2 bytes (16 bits) or 4 bytes (32 bits) at once:

| `data_write_n` | Meaning |
|---|---|
| `11` | Not writing |
| `00` | Writing 8 bits |
| `01` | Writing 16 bits |
| `10` | Writing 32 bits |

If your registers are only 8 bits wide, you can treat any value other than `11` as "a write is happening".

## What a write looks like, tick by tick

```
clk          ‾|_|‾|_|‾|_|‾|_
address      ══╳ 0x00 ╳═══════
data_in      ══╳ 120  ╳═══════
data_write_n 11╳  00  ╳ 11 ───
X_POS              ───╳ 120 ──   ← your register picks it up on the clock tick
```

In Verilog that's:

```verilog
always @(posedge clk) begin
    if (!rst_n)
        x_pos <= 0;                          // reset: back to the start value
    else if (address == 6'h00 && data_write_n != 2'b11)
        x_pos <= data_in[7:0];               // a write to box 0x00: store it
end
```

## What a read looks like

Reads are simpler. `data_out` always shows the box the CPU is pointing at:

```verilog
assign data_out = (address == 6'h00) ? {24'b0, x_pos} :
                  (address == 6'h04) ? {24'b0, y_pos} :
                  32'h0;                     // any other box reads as 0
assign data_ready = 1;                       // answer is always ready straight away
```

## Where your peripheral lives on the chip

The chip has numbered **slots**. OR gives each peripheral a slot, and from then on the game reaches your registers at **slot base address + register offset**. The game team copies your register table from `SPEC.md` into `briss_regs.h`, so the C code knows the addresses.

**That's why your `SPEC.md` register table must be exact.** If the spec says BUTTONS is at 0x00 but your Verilog uses 0x04, the game reads the wrong box and nothing works.

## Next

- See all of this in real code: [example-walkthrough.md](example-walkthrough.md)
- Words you don't know: [glossary.md](glossary.md)
