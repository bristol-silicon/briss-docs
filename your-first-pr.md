# Your first pull request

Everyone does this once, to practise the workflow before touching any Verilog. It takes about 15 minutes.

**What you'll do:** add a file with your GitHub username to `practice/` in this repo (briss-docs), by going through the full branch → pull request → review → merge loop.

**Before you start:**
- You've accepted your invite to the `bristol-silicon` organisation (check your email or github.com/bristol-silicon).
- You've followed [setup.md](setup.md). Codespaces is easiest.

## The steps

### 1. Open briss-docs in Codespaces
`github.com/bristol-silicon/briss-docs` → **Code** → **Codespaces** → **Create codespace on main**.
(Or on your laptop: `git clone https://github.com/bristol-silicon/briss-docs.git && cd briss-docs`.)

### 2. Make a branch
A **branch** is your own copy to work on, so you can't break `main`.
```bash
git switch main
git pull
git switch -c yourname/first-pr
```

### 3. Make your change
Create the file `practice/<your-github-username>.md` containing one line:
```
Hi, I'm <first name> from Team <AND/OR/XOR/NAND/NOR/NOT> and I want to learn <one thing>.
```

### 4. Save a snapshot (commit)
```bash
git add practice/
git commit -m "Add <your-github-username> to practice"
```

### 5. Upload your branch (push)
```bash
git push -u origin yourname/first-pr
```

### 6. Open a pull request
GitHub shows a yellow banner: **Compare & pull request**. Click it. (Or go to the repo → **Pull requests** → **New pull request** → choose your branch.)
- Title: `Add <your-github-username> to practice`
- Fill in the checklist. Most of it doesn't apply here, which is fine.
- Click **Create pull request**.

### 7. Get a review
Ask someone in your team to review it. They:
1. Open your pull request → **Files changed**.
2. Check it's just your one file.
3. Click **Review changes** → **Approve** → **Submit review**.

**You review someone else's too.** Reviewing is half of the skill.

### 8. Merge
Once it's approved, the **Merge pull request** button turns green. Click it, then **Delete branch**.

Done 🎉 You've just done exactly what you'll do for every change to the chip.

## What just happened

```
main ──●────────────────────●──   (the official version)
        \                  /
         ●── your commit ─●       (your branch → pull request → review → merge)
```

- You never changed `main` directly. You can't: GitHub blocks it.
- Someone else checked your work before it went in.
- In the peripheral repos, GitHub also **runs the tests automatically** on every pull request. You'll see ✅ or ❌ next to it, and a ❌ blocks the merge.

## If something went wrong

| What you see | What to do |
|---|---|
| `error: failed to push` / permission denied | Your org invite isn't accepted yet, or you don't have write access. Ask OR |
| "Merge" button is grey | It needs an approval first, or the checks are still running |
| You committed to `main` by mistake | Don't push. Run `git switch -c yourname/first-pr` and push that branch instead |
| Merge conflict | Ask in your team chat. Someone will help you through it the first time |

Next: [example-walkthrough.md](example-walkthrough.md) to understand the Verilog you'll be editing.
