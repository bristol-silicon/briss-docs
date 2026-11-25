# Glossary

Every term you'll hear at BriSS, in plain English, roughly grouped.

## The chip

| Term | Meaning |
|---|---|
| **ASIC** | A chip designed for one job and manufactured in silicon. Ours is one |
| **Tape-out** | Sending the finished design to be manufactured. Ours is April 2027 |
| **Tiny Tapeout** | The project that lets students get small designs made cheaply, by sharing one chip between many designs |
| **Tile** | One unit of chip area on Tiny Tapeout. Our design is 3×4 tiles |
| **GF180** | The manufacturing process we use: GlobalFoundries, 180 nanometre |
| **PDK** | Process Design Kit: the rules and building blocks for a specific factory process |
| **GDS** | The final chip layout file: the actual shapes that get printed on silicon |
| **RISC-V** | An open standard for CPU instructions. Our CPU understands it |
| **TinyQV** | The small RISC-V CPU (by Michael Bell) at the heart of our chip |
| **Peripheral** | A helper block next to the CPU that does one job: video, audio, buttons... Each team builds these |
| **Slot** | A numbered position on the chip where a peripheral plugs in. OR assigns them |
| **FPGA** | A reprogrammable chip. We use it to test the whole design for real before tape-out |

## Hardware design

| Term | Meaning |
|---|---|
| **RTL** | Register Transfer Level: hardware described in code. Our Verilog is RTL |
| **Verilog** | The language we describe hardware in |
| **Module** | A block of hardware with inputs and outputs: Verilog's "function" |
| **Register** (hardware) | A small piece of memory that holds a value between clock ticks |
| **Register** (peripheral) | A numbered "post box" the CPU can write to or read from. See [how-peripherals-work.md](how-peripherals-work.md) |
| **Address / offset** | The number of a register, e.g. 0x04. "Offset" means relative to the start of your peripheral |
| **Bus** | The shared wires the CPU uses to talk to all peripherals (address, data, read/write) |
| **Clock (`clk`)** | A steady tick (24 MHz on our chip) that keeps everything in step |
| **Reset (`rst_n`)** | Puts everything back to its starting value. `_n` = active when 0 |
| **Combinational** | Logic with no memory; the output depends only on the current inputs (`assign`) |
| **Sequential** | Logic with memory; it updates on clock ticks (`always @(posedge clk)`) |
| **`<=` vs `=`** | `<=` updates on the clock tick (use in clocked blocks). `=` is immediate (use in combinational blocks) |
| **Latch** | Accidental memory, caused by not giving an output a value on every path. Avoid |
| **Lint** | An automatic check for common Verilog mistakes. Runs on every pull request |
| **Interrupt** | A signal from a peripheral saying "CPU, something happened!" |
| **Hex (`0x1F`, `8'h1F`)** | Base-16 numbers. `8'h1F` in Verilog means an 8-bit value of hex 1F (= 31) |
| **VGA** | The video signal standard our video peripheral produces |

## Testing

| Term | Meaning |
|---|---|
| **Testbench** | Code that tests your design by driving inputs and checking outputs |
| **cocotb** | The Python library we write tests in |
| **Simulation** | Running your Verilog on a computer to see what it would do |
| **Icarus Verilog** | The simulator our tests use |
| **Waveform / VCD** | A recording of every signal over time, viewed in Surfer or GTKWave |
| **`assert`** | "This must be true, or the test fails" |
| **T1, T2...** | The numbered checks in your SPEC.md. Each becomes one test |

## GitHub and Git

| Term | Meaning |
|---|---|
| **Git** | The tool that tracks every change to the files |
| **GitHub** | The website where our repos live |
| **Organisation (org)** | The society's account: `bristol-silicon` |
| **Repo (repository)** | One project folder with its full history |
| **Clone** | Download a copy of a repo |
| **Branch** | Your own copy to work on without touching `main` |
| **`main`** | The official version. Protected: changes only arrive through reviewed pull requests |
| **Commit** | A saved snapshot of your changes, with a message |
| **Push / pull** | Upload your commits to GitHub / download others' commits |
| **Pull request (PR)** | Asking for your branch to be merged into `main` |
| **Review** | A teammate checks your PR and approves it or asks for changes |
| **Merge** | Your branch's changes join `main` |
| **Issue** | A task or bug, tracked on GitHub |
| **Milestone** | A deadline that groups issues: Spec freeze, v0.1, v0.2, v1.0 |
| **Checks / CI** | Tests GitHub runs automatically on every PR (✅ or ❌) |
| **Codespaces** | VS Code in your browser with everything installed |

## BriSS-specific

| Term | Meaning |
|---|---|
| **SPEC.md** | Your peripheral's plan: purpose, pins, registers, behaviour, checks |
| **`briss_regs.h`** | The C file listing every peripheral's register addresses, copied by hand from each SPEC.md |
| **Spec freeze** | 11 Dec: register tables are locked. Changes after this need a `spec-change` issue |
| **v0.1 / v0.2 / v1.0** | The version milestones. See the [README](README.md) |
| **Size budget** | How much chip area your peripheral may use, set by OR |
