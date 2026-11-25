# Start here

New to BriSS, chips or GitHub? Read this page first. It takes 10 minutes.

## What we're building

We're building a **real chip**: a tiny games console on silicon.

```
          ┌──────────────────────────── our chip ────────────────────────────┐
 buttons ─┤                                                                   │
 knob ────┤   ┌─────────┐     the bus      ┌──────────┐  ┌──────────┐        ├─ VGA → screen
          │   │  TinyQV │◄────────────────►│  video   │  │  audio   │ ...    ├─ sound → speaker
          │   │  RISC-V │  (addresses +    │  (XOR)   │  │  (NAND)  │        │
          │   │   CPU   │   data wires)    └──────────┘  └──────────┘        │
          │   └────┬────┘                  ┌──────────┐  ┌──────────┐        │
          │        │ runs                  │ gamepad  │  │  random  │ ...    │
          │   ┌────┴────┐                  │  (NOR)   │  │  (NOT)   │        │
          │   │ the game│                  └──────────┘  └──────────┘        │
          │   │ (AND, C)│                                                    │
          │   └─────────┘                                                    │
          └───────────────────────────────────────────────────────────────────┘
```

- **The CPU** (TinyQV, already designed by Michael Bell) runs a **game written in C** (team AND).
- The CPU can't draw on a screen or make sound by itself. It needs helpers called **peripherals**. Each team builds one or more of them:

| Team | Peripheral | What it does for the game |
|---|---|---|
| XOR | Video | Draws the player, enemies and score on a VGA screen |
| NAND | Audio | Plays tones, noise and music |
| NOR | Gamepad, rotary knob, score | Reads the controls, shows the score |
| NOT | Random, collision, enemy grid | Random numbers, "did the bullet hit?", the grid of enemies |
| AND | Maths, plus the game itself | Fast maths helpers, and the C code that ties it all together |
| OR | Integration | Plugs everything into the chip, tests it on an FPGA, builds the final layout |

In **April 2027** the finished design goes to **Tiny Tapeout**, who manufacture it on a real silicon wafer and send us back working chips.

## How the CPU talks to your peripheral

This is the key idea. Read [how-peripherals-work.md](how-peripherals-work.md) for the full version.

> The CPU **writes a number to an address**. Your peripheral **stores it** and does something with it.
> The CPU **reads from an address**. Your peripheral **answers** with a number.

That's all a peripheral is: a few numbered boxes (**registers**) that the CPU can write to and read from, plus the logic that does something with them.

## The journey from idea to chip

```
SPEC.md  →  Verilog  →  tests  →  FPGA  →  chip layout (GDS)  →  real silicon
 (plan)      (design)   (check)   (try it    (OR does this)       (April)
                                   for real)
```

1. **Spec:** agree what your peripheral does and which registers it has. Written in `SPEC.md`.
2. **Verilog:** describe the hardware. You learnt this at the VULKOS session.
3. **Tests:** a Python script pretends to be the CPU and checks your design gives the right answers.
4. **FPGA:** run the whole chip on a reprogrammable board to see the game working.
5. **GDS:** the final layout of the chip, built automatically by OR's tools.

## Your first three steps

1. **Get set up:** follow [setup.md](setup.md). The easiest way is one click in your browser.
2. **Practise the workflow:** follow [your-first-pr.md](your-first-pr.md). No Verilog needed.
3. **See a real peripheral:** read [example-walkthrough.md](example-walkthrough.md), which goes through the example peripheral line by line.

Then open your team's repo, read its `SPEC.md`, and pick a task.

Stuck on a word? See the [glossary](glossary.md).
