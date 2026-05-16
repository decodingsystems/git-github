# Lab 07: Stashing Work in Progress

## Objective
Learn to use `git stash` to save incomplete work and switch contexts without making a messy commit.

## Scenario
You're halfway through developing a feature when your manager asks you to hotfix a bug on `main` immediately. You can't commit half-finished code. Stash saves your work-in-progress.

## Steps

### 1. Setup
```bash
mkdir stash-demo && cd stash-demo
git init
echo "v1.0 production code" > app.js
git add . && git commit -m "Initial production code"
```

### 2. Start Working on a Feature (Don't Finish)
```bash
git checkout -b feature/dark-mode
echo "// dark mode in progress" >> app.js
echo ".dark { color: white; }" > styles.css
# Oops — urgent bug fix needed on main!
git status   # You have unstaged and untracked changes
```

### 3. Stash Your Work
```bash
git stash push -m "WIP: dark mode feature"
git status   # Clean working directory!
ls           # styles.css is gone — it's safely in the stash
```

### 4. Switch to Main and Fix the Bug
```bash
git checkout main
echo "// hotfix: null check added" >> app.js
git add . && git commit -m "hotfix: add null check"
```

### 5. Return to Feature Branch and Restore Stash
```bash
git checkout feature/dark-mode
git stash list          # Shows all stashes
git stash pop           # Applies stash and removes it from the list
git status              # Your changes are back!
cat app.js              # WIP comment is there
ls                      # styles.css is back
```

### 6. Working with Multiple Stashes
```bash
echo "another change" >> app.js
git stash push -m "WIP: extra change"

echo "third change" >> app.js
git stash push -m "WIP: third change"

git stash list          # stash@{0}, stash@{1}, stash@{2}
git stash apply stash@{1}   # Apply a specific stash (keeps it in list)
git stash drop stash@{1}    # Delete a specific stash
git stash clear             # Delete ALL stashes
```

### 7. Stash Including Untracked Files
New files (untracked) are NOT stashed by default. Use `-u` flag:
```bash
touch newfile.txt
git stash push -u -m "WIP: with untracked"
ls   # newfile.txt is gone too
git stash pop
ls   # newfile.txt is back
```

## Key Takeaways
- `git stash` = a stack of saved snapshots. LIFO (Last In, First Out) with `pop`.
- `pop` = apply + delete from stack. `apply` = apply + keep in stack.
- Always add `-m` messages to your stashes — `stash@{2}` is meaningless later.
- `-u` flag includes untracked (new) files in the stash.
