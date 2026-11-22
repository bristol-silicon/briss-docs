# Teams and repos

Each repo has **two owners** named in its README. Owners approve pull requests, but anyone can open one.

| Team | Builds | Repos |
|---|---|---|
| **AND** | The game (C on TinyQV), the register header, maths helpers | [briss-game](https://github.com/bristol-silicon/briss-game), [periph-maths](https://github.com/bristol-silicon/periph-maths) |
| **OR** | Integration, chip build, FPGA, test tools; tags versions | [briss-chip](https://github.com/bristol-silicon/briss-chip), [briss-peripheral-template](https://github.com/bristol-silicon/briss-peripheral-template) |
| **XOR** | Video: VGA timing, sprites, text overlay | [periph-video](https://github.com/bristol-silicon/periph-video) |
| **NAND** | Audio: tones, noise, sequencer | [periph-audio](https://github.com/bristol-silicon/periph-audio) |
| **NOR** | Inputs and display: gamepad, rotary encoder, score | [periph-gamepad](https://github.com/bristol-silicon/periph-gamepad), [periph-rotary](https://github.com/bristol-silicon/periph-rotary), [periph-score](https://github.com/bristol-silicon/periph-score) |
| **NOT** | Game hardware: random numbers, collision, enemy grid (inside periph-video) | [periph-random](https://github.com/bristol-silicon/periph-random), [periph-collision](https://github.com/bristol-silicon/periph-collision) |

## Sub-groups

| Team | Sub-groups |
|---|---|
| AND | Game logic · Drivers + header (`briss_regs.h`) · Maths |
| OR | Integration + checks · FPGA + test tools |
| XOR | VGA timing · Sprite · Text overlay |
| NAND | Tone + audio out · Noise · Sequencer |
| NOR | Gamepad · Rotary · Score |
| NOT | Random + collision · Enemy grid |

Member lists are kept in the committee's roster, not in this public repo.
