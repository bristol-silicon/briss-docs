# Example peripheral: line by line

Every peripheral repo starts with a small working example in `src/peripheral.v`, with tests in `test/test.py`. This page explains both. Once you understand these two files, you understand how every BriSS peripheral works.

Read [how-peripherals-work.md](how-peripherals-work.md) first.

## What the example does

| Address | Read | Write |
|---|---|---|
| 0x00 | The stored 32-bit value | Store a new value |
| 0x04 | The current input pins (`ui_in`) | Nothing |
| 0x08 | 0 | Writing 1 clears the interrupt |

The output pins show `stored value + input pins`, and an interrupt fires when input pin 6 goes high.

## `src/peripheral.v`

### 1. The module and its wires

```verilog
module tqvp_example (
    input         clk,
    input         rst_n,
    input  [7:0]  ui_in,
    output [7:0]  uo_out,
    input  [5:0]  address,
    input  [31:0] data_in,
    input  [1:0]  data_write_n,
    input  [1:0]  data_read_n,
    output [31:0] data_out,
    output        data_ready,
    output        user_interrupt
);
```

A **module** is a box with inputs and outputs. These are the standard wires every peripheral has (see the table in how-peripherals-work.md). **Don't change this list.** Do rename the module to `tqvp_briss_<yourname>` and update `src/tt_wrapper.v` to match.

### 2. A register you can write

```verilog
reg [31:0] example_data;                       // a 32-bit box

always @(posedge clk) begin                    // on every clock tick...
    if (!rst_n) begin
        example_data <= 0;                     // ...if resetting, empty the box
    end else begin
        if (address == 6'h0) begin             // ...if the CPU is talking to box 0
            if (data_write_n != 2'b11)              example_data[7:0]   <= data_in[7:0];
            if (data_write_n[1] != data_write_n[0]) example_data[15:8]  <= data_in[15:8];
            if (data_write_n == 2'b10)              example_data[31:16] <= data_in[31:16];
        end
    end
end
```

- `always @(posedge clk)` means "do this on every rising clock edge". This is how hardware **remembers** things.
- `<=` means "update on the clock tick". Always use it inside `always @(posedge clk)`.
- The three `if`s handle 8, 16 and 32-bit writes:
  - **Any write** (`!= 11`) updates the bottom byte.
  - **A 16 or 32-bit write** (`01` or `10`, where the two bits differ) also updates the second byte.
  - **Only a 32-bit write** (`10`) updates the top half.

  If your registers are 8 bits, you only need the first line.

### 3. Output pins

```verilog
assign uo_out = example_data[7:0] + ui_in;
```

`assign` is **combinational** logic: it's always true, with no clock involved. Here the output pins show the stored byte plus whatever is on the input pins. In your peripheral this is where you'd drive VGA, sound and so on.

### 4. Answering reads

```verilog
assign data_out = (address == 6'h0) ? example_data :
                  (address == 6'h4) ? {24'h0, ui_in} :
                  32'h0;

assign data_ready = 1;
```

- `? :` means "if ... then ... else". So: box 0 answers with the stored value, box 4 answers with the input pins, and everything else answers 0.
- `{24'h0, ui_in}` glues 24 zero bits on top of the 8 input bits to make 32 bits.
- `data_ready = 1` means "my answer is always ready immediately". Keep it like this unless you know you need longer.

### 5. The interrupt (optional)

```verilog
reg example_interrupt;
reg last_ui_in_6;

always @(posedge clk) begin
    if (!rst_n) example_interrupt <= 0;

    if (ui_in[6] && !last_ui_in_6)                  // pin 6 just went from 0 to 1
        example_interrupt <= 1;                     // raise the flag
    else if (address == 6'h8 && data_write_n != 2'b11 && data_in[0])
        example_interrupt <= 0;                     // CPU wrote 1 to box 8: clear it

    last_ui_in_6 <= ui_in[6];                       // remember pin 6 for next tick
end

assign user_interrupt = example_interrupt;
```

This is how you detect a **rising edge**: compare the input now with what it was last tick. Most BriSS peripherals won't need interrupts; the game can just read a status register.

### 6. Tidying up

```verilog
wire _unused = &{data_read_n, 1'b0};
```

`data_read_n` isn't used here, so this line stops the lint check complaining. If you don't use an input, add it to this list.

## `test/test.py`

The test **pretends to be the CPU**. It writes and reads registers and checks the answers.

```python
@cocotb.test()
async def test_project(dut):
    clock = Clock(dut.clk, 100, units="ns")      # start a clock
    cocotb.start_soon(clock.start())

    tqv = TinyQV(dut, PERIPHERAL_NUM)            # a pretend CPU
    await tqv.reset()                            # reset everything

    await tqv.write_word_reg(0, 0x82345678)      # CPU writes 32 bits to box 0
    assert await tqv.read_byte_reg(0)  == 0x78   # reading 8 bits gives the bottom byte
    assert await tqv.read_word_reg(0)  == 0x82345678   # reading 32 bits gives it all

    dut.ui_in.value = 30                         # put 30 on the input pins
    await ClockCycles(dut.clk, 3)                # wait (inputs take 2 ticks to arrive)
    assert dut.uo_out.value == 0x96              # 0x78 + 30 = 0x96 on the output pins

    assert await tqv.read_byte_reg(4) == 30      # box 4 reads the input pins
    assert await tqv.read_word_reg(8) == 0       # box 8 reads 0
```

- `await` means "wait for this to finish". Reads and writes take a few clock ticks.
- `assert` means "this must be true, or the test fails". **Each check in your SPEC.md becomes one test like this.**
- `tqv.write_byte_reg / write_hword_reg / write_word_reg` write 8 / 16 / 32 bits. The read versions read them back.

### Running it

```bash
cd test
make
```

You'll see `PASS` or `FAIL` and, if it failed, which `assert` broke. A waveform file, `tb.vcd`, is also saved. Open it in **Surfer** (installed in Codespaces) or GTKWave to see every wire, tick by tick.

## Turning the example into your peripheral

1. Fill in `SPEC.md`: your registers and what they do.
2. Rename the module to `tqvp_briss_<name>`, and update `tt_wrapper.v` line 41.
3. Replace `example_data` with your registers: one `reg` and one write rule each.
4. Change `data_out` so each address answers with the right register.
5. Write one test per check in your spec. Ideally a teammate writes them.
6. Delete what you don't need, e.g. the interrupt.

That's v0.1: **every register exists and reads back correctly**.
