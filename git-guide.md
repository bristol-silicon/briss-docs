# Git in 10 minutes

## One-time setup

1. Make a GitHub account and send your username to your team's WhatsApp so OR can add you.
2. Install Git, then tell it who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@bristol.ac.uk"
```

## The loop you'll do every session

```bash
git switch main
git pull                          # 1. get the latest
git switch -c yourname/what-im-doing   # 2. new branch
# 3. edit files, run the tests: cd test && make
git add -A
git commit -m "Say what you changed"   # 4. save a snapshot
git push -u origin yourname/what-im-doing   # 5. upload
```

6. On GitHub, click **Compare & pull request**, fill in the checklist and write `Closes #<issue number>`.
7. Wait for the green tick and a review. Fix anything, then push again to the same branch.

## Words

- **Repo**: a project folder with its full history.
- **Branch**: your own copy to work on without breaking `main`.
- **Commit**: a saved snapshot with a message.
- **Pull request (PR)**: asking for your branch to be merged into `main`.
- **Issue**: a task or bug.

## Stuck?

`git status` tells you where you are. If it's a mess, ask in WhatsApp before deleting anything.
