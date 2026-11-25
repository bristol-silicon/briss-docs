# Writing your spec

Your team's first job is your peripheral's `SPEC.md`. It's due at the **spec freeze on 11 Dec**.

You don't need to know any Verilog to write a spec. It's a plan: **what the game can tell your peripheral, what it can ask it, and what it does on its own.** If you can answer the questions below, you can write a spec.

Read [how-peripherals-work.md](how-peripherals-work.md) first, so "register" and "address" make sense.

## Step 1: answer these questions on paper (about 20 minutes, as a team)

| # | Question | Your answers become... |
|---|---|---|
| 1 | What does the game need to **tell** your peripheral? (settings, positions, notes to play...) | **Write** registers (R/W) |
| 2 | What does the game need to **ask** your peripheral? (button states, results, counts...) | **Read** registers (R) |
| 3 | What does your peripheral do **on its own**, without the CPU asking? (counting, drawing the screen, playing a tone...) | §4 Behaviour |
| 4 | What **events** does the game need to hear about? ("a button was just pressed", "the sound finished") How does the game say "got it"? | Status flags, and how they're cleared |
| 5 | What should everything be **after reset**? | Reset values |
| 6 | Which **pins** to the outside world does it need? (buttons in, VGA out...) | §2 Pins |
| 7 | What's the **smallest version that's still useful**? | v0.1 / v0.2 |
| 8 | How will you **prove it works**? | §5 Checks (one test each) |

Don't skip question 2. Beginners often design only the "tell" side and forget the game needs to read things back.

## Step 2: turn the answers into a register table

Rules of thumb:

- **One register per idea.** Don't pack unrelated things together just to save addresses.
- **Use word-aligned offsets:** `0x00`, `0x04`, `0x08`, `0x0C`... You have up to 16 registers (`0x00`–`0x3C`).
- **Give the units** in the Meaning column: clock ticks, pixels, Hz, frames. "Speed" on its own is useless; "pixels moved per frame" isn't.
- **Use only the bits you need.** Every stored bit costs chip area, and area is our tightest limit. Unused bits read as 0.
- **Names in CAPITALS**, short, with no spaces: `BUTTONS`, `TONE_FREQ`, `SPRITE_X`.
- **Say what writing a read-only register does** (usually nothing) and what reading a write-only one gives (usually 0).
- **Flags the game clears:** say how. "Write 1 to clear" is the usual way.

## Step 3: look at the worked example

[example-spec.md](example-spec.md) is a complete spec for a **made-up timer peripheral**. It isn't one of ours, so copy its *style* but not its contents. Here's how its answers to the questions turned into registers:

| Question | Timer's answer | Became |
|---|---|---|
| 1. Tell | "Count down from this number", "start", "stop", "repeat?" | `RELOAD` (R/W), `CTRL` (R/W) |
| 2. Ask | "How far through are you?" | `COUNT` (R) |
| 3. On its own | Counts down by 1 every clock tick | Behaviour section |
| 4. Events | "I've reached zero" | `STATUS.DONE`, write 1 to clear |
| 5. Reset | Everything 0, stopped | Reset value column |
| 6. Pins | None | "No pins" |
| 7. Smallest | Registers that just store and read back | v0.1 |
| 8. Prove | 8 checks | T1–T8 |

## Step 4: get it reviewed

1. Fill in `SPEC.md` in your repo, on a branch (e.g. `yourname/spec`).
2. Open a pull request titled `Spec: first draft`.
3. Ask for reviews from:
   - **AND** (the game team): "Could you write the game with these registers?"
   - **OR** (integration): pins, slot and size.
   - **The technical lead**, who gives the final approval.
4. Fix what they ask for, and merge **before 11 Dec**.

A first draft that's 80% right and reviewed is much better than a perfect one that's late. "Open questions" (§8) is the right place for anything you're unsure about.

## Common mistakes

| Mistake | Instead |
|---|---|
| The register table and the description disagree | The table is the official version. Fix the text to match it |
| "The game sets the speed" | "The game writes pixels-per-frame to `SPEED` (0x04, bits 3:0)" |
| No reset values | Every register needs one, even if it's 0 |
| A check you can't test, e.g. "works correctly" | "After writing 5 to `X`, reading `X` returns 5" |
| Planning a huge feature list | Plan v1.0 small. You can't add features after the design freeze, but you can always drop them |
