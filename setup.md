# Setup

You need three things: **Git**, **Python with cocotb**, and **Icarus Verilog** (the simulator). There are two ways to get them.

## Option A: in your browser, nothing to install (recommended)

GitHub **Codespaces** gives you VS Code in your browser with everything already installed. It works on any laptop, including Windows, Mac and Chromebooks.

1. Open your team's repo on GitHub, e.g. `bristol-silicon/periph-audio`.
2. Click the green **Code** button → **Codespaces** tab → **Create codespace on main**.
   - If you get the option, pick the **4-core** machine. The 2-core one may be too small for the toolset.
3. Wait for it to build. **The first start takes 5–10 minutes**; after that it opens in seconds.
4. In the terminal at the bottom, run:
   ```bash
   cd test
   make
   ```
   You should see `PASS`. You're set up.

**Cost:** Codespaces is free up to a monthly limit, which is plenty for weekly sessions. Students can get more free hours, plus other perks, through the [GitHub Student Developer Pack](https://education.github.com/pack).

**Stop your codespace when you're done** (the **Codespaces** menu bottom-left → *Stop current codespace*) so you don't use up your free hours.

## Option B: on your own laptop

### Mac
```bash
# Install Homebrew first if you don't have it: https://brew.sh
brew install git icarus-verilog python
```

### Windows
Use **WSL** (a Linux terminal built into Windows):
1. Open PowerShell as administrator and run `wsl --install`, then restart.
2. Open the **Ubuntu** app and run:
   ```bash
   sudo apt update
   sudo apt install -y git iverilog python3 python3-pip python3-venv
   ```

### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install -y git iverilog python3 python3-pip python3-venv
```

### Then, on all systems
```bash
git clone https://github.com/bristol-silicon/<your-team-repo>.git
cd <your-team-repo>
python3 -m venv .venv
source .venv/bin/activate          # do this each time you open a new terminal
pip install -r test/requirements.txt
cd test
make
```
You should see `PASS`.

### Seeing waveforms
`make` saves a waveform to `test/tb.vcd`. To view it:
- **Codespaces:** right-click `tb.vcd` → open with **Surfer**.
- **Laptop:** install [GTKWave](https://gtkwave.sourceforge.net/) (`brew install --cask gtkwave` on Mac, `sudo apt install gtkwave` on Linux/WSL) and run `gtkwave tb.vcd`.

## Set up Git once

Tell Git who you are, using the email on your GitHub account:
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Common problems

| Problem | Fix |
|---|---|
| `iverilog: command not found` | Icarus isn't installed. Use Codespaces, or redo the install step |
| `ModuleNotFoundError: cocotb` | Run `source .venv/bin/activate` first, then `pip install -r test/requirements.txt` |
| `git push` asks for a password | GitHub needs a token, not your password. Use Codespaces, or install the [GitHub CLI](https://cli.github.com/) and run `gh auth login` |
| `Permission denied` when pushing | You haven't been added to the org yet, or your invite isn't accepted. Ask OR |
| Codespace won't start | Try the 4-core machine, or ask in your team chat |

Still stuck after 20 minutes? Ask in your team's WhatsApp, with a screenshot of the error.
