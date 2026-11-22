# Verilog coding guide

Short on purpose. If a rule gets in your way, ask OR.

1. **One clock.** Everything uses `clk`. No generated clocks, no `always @(posedge some_signal)`.
2. **Reset like the template.** Use `rst_n` (active low) and reset every register that the spec gives a reset value.
3. **No latches.** In `always @(*)` blocks, give every output a value on every path (set defaults at the top).
4. **`<=` in clocked blocks, `=` in combinational blocks.** Never mix them in one block.
5. **No warnings.** The lint check in GitHub Actions must be green.
6. **Name things after the spec.** If the spec says `BUTTONS`, the register is `buttons_reg`, not `r3`.
7. **Comment the why, not the what.** `// cleared on read so the game sees each press once` beats `// set to 0`.
8. **No `initial` blocks** in `src/` (they don't exist on a real chip). They're fine in tests.
9. **Watch your size.** Multipliers, dividers and big memories are expensive. Check with OR before using them.
10. **One test per check.** Each `T` number in `SPEC.md` gets one cocotb test, written by someone who didn't write the Verilog.
