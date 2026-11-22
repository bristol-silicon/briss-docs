# Reviewing a pull request

Aim for 10 minutes. Be kind and specific.

- [ ] The checks are green (tests and lint)
- [ ] It does what the linked issue asked, and nothing extra
- [ ] Registers match `SPEC.md` exactly (offset, bits, reset value)
- [ ] If registers changed: `SPEC.md` and `briss_regs.h` are both updated, and there's an approved `spec-change` issue (after the freeze)
- [ ] Tests check real behaviour, not just "it didn't crash"
- [ ] The test was written by someone other than the Verilog author
- [ ] Follows the [coding guide](coding-guide.md)

Then: **Approve**, or **Request changes** with a clear list of what to fix.
