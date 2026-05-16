# Lab 08: Interactive Rebase — Clean Up Commit History

## Objective
Use `git rebase` and interactive rebase to maintain a clean, linear commit history before pushing.

## Why Rebase?
Merging creates a merge commit and preserves the full branch history (good for long-lived branches). Rebasing **rewrites** commits to sit on top of the target branch, creating a perfectly linear history. It's preferred for feature branches before merging into main to keep history readable.

## Part 1: Basic Rebase
```bash
mkdir rebase-demo && cd rebase-demo
git init
echo "main line 1" > main.txt && git add . && git commit -m "Initial commit"
echo "main line 2" >> main.txt && git add . && git commit -m "Second commit on main"

# Create feature branch from first commit
git checkout -b feature/clean HEAD~1
echo "feature content" > feature.txt
git add . && git commit -m "Add feature"

git log --oneline --graph --all
# Feature branched off before the "second commit on main" — it's behind
```

### Rebase onto main
```bash
git rebase main
git log --oneline --graph --all
# Feature's commit is now on top of main's latest commit — linear history!
```

## Part 2: Interactive Rebase — Squash, Reword, Drop
You've been committing frequently with messy messages. Before pushing, clean it up.

```bash
git checkout main
# Create 5 messy commits
echo "step 1" >> notes.txt && git add . && git commit -m "wip"
echo "step 2" >> notes.txt && git add . && git commit -m "wip"
echo "step 3" >> notes.txt && git add . && git commit -m "fixed typo"
echo "step 4" >> notes.txt && git add . && git commit -m "asdf"
echo "step 5" >> notes.txt && git add . && git commit -m "done"

git log --oneline    # 5 messy commits + initial commits
```

### Launch Interactive Rebase for last 5 commits
```bash
git rebase -i HEAD~5
```

An editor opens showing:
```
pick abc1234 wip
pick def5678 wip
pick ghi9012 fixed typo
pick jkl3456 asdf
pick mno7890 done
```

Change the commands:
```
pick abc1234 wip
squash def5678 wip          # squash into previous commit
squash ghi9012 fixed typo   # squash into previous
squash jkl3456 asdf         # squash into previous
reword mno7890 done         # keep but edit the message
```
- **pick**: Use this commit as-is.
- **squash (s)**: Merge into the previous commit.
- **reword (r)**: Keep commit but edit its message.
- **drop (d)**: Completely delete this commit.

Save and close. Git will prompt for the squash commit message. Write a clean message like `"feat: add 5-step notes feature"`.

```bash
git log --oneline    # Now just 1 clean commit instead of 5!
```

## ⚠️ Warning
**Never rebase commits that have already been pushed to a shared remote branch.** Rebasing rewrites SHAs, which will conflict with teammates' copies.

## Key Takeaways
- `git rebase main` = replay your branch commits on top of main's current HEAD.
- `git rebase -i HEAD~N` = interactive rebase of the last N commits.
- Use squash to compress "WIP" commits into clean, meaningful units before PRs.
