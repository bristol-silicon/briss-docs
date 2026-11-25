# BriSS chip docs

Everything about the **Bristol Silicon Society** chip in one place: a RISC-V (TinyQV) games chip with peripherals built by six teams, taped out on the Tiny Tapeout GF180 shuttle (TTGF27a, April 2027).

## New here? Read these in order

1. [start-here.md](start-here.md): what we're building and how it fits together
2. [setup.md](setup.md): get the tools working (one click with Codespaces)
3. [your-first-pr.md](your-first-pr.md): practise the GitHub workflow
4. [how-peripherals-work.md](how-peripherals-work.md): registers, addresses and the bus, in plain English
5. [example-walkthrough.md](example-walkthrough.md): the example peripheral, line by line
6. [writing-your-spec.md](writing-your-spec.md): how to write your team's SPEC.md, with a worked [example](example-spec.md)
7. [glossary.md](glossary.md): every term in one place

## Reference

| Read this | When |
|---|---|
| [system-spec.md](system-spec.md) | What the whole chip does: pins, clock, slots, size budgets |
| [teams.md](teams.md) | Which team owns which repo |
| [git-guide.md](git-guide.md) | First time using Git? Start here |
| [coding-guide.md](coding-guide.md) | Rules for writing Verilog |
| [review-checklist.md](review-checklist.md) | Reviewing someone's pull request |
| [decision-log.md](decision-log.md) | Big decisions and why we made them |

## Timeline

| When | Milestone |
|---|---|
| Nov 2026 | Teams formed; specs drafted |
| Mid Dec 2026 | **Spec freeze**: register tables locked (changes need a `spec-change` issue) |
| Before Christmas | v0.1 stubs of every peripheral pass T1 |
| Mid Feb 2027 | v0.2: first real behaviour; first full-chip FPGA build |
| Mid Mar 2027 | **Design freeze**: v1.0 of everything, under size budget |
| April 2027 | Tape-out on TTGF27a |
