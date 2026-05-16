# Lab 10: Undoing Mistakes — Reset, Revert, Restore

## Objective
Safely undo changes at different stages (working dir, staging, committed) using the right Git command.

## Decision Tree for Undoing
```
"I made a mistake and want to undo..."
│
├── Changes NOT yet staged (working dir only)
│     └── git restore <file>
│
├── Changes staged but NOT committed yet
│     └── git restore --staged <file>     (unstage only)
│     └── git restore --staged --worktree <file>  (unstage + discard)
│
├── Last commit (not yet pushed to remote)
│     ├── Keep changes: git reset --soft HEAD~1
│     ├── Unstage changes: git reset --mixed HEAD~1 (default)
│     └── Discard changes: git reset --hard HEAD~1 ⚠️
│
└── Commit already pushed / shared
      └── git revert <SHA>   (creates a new "undo" commit — safe!)
```

## Full Lab

### Setup
```bash
mkdir undo-demo && cd undo-demo
git init
echo "line 1" > file.txt && git add . && git commit -m "Commit 1"
echo "line 2" >> file.txt && git add . && git commit -m "Commit 2"
echo "line 3" >> file.txt && git add . && git commit -m "Commit 3"
git log --oneline   # 3 commits
```

### 1. Restore (Discard Unstaged Changes)
```bash
echo "BAD CHANGE" >> file.txt
git status     # Modified, unstaged
git restore file.txt
cat file.txt   # BAD CHANGE is gone!
```

### 2. Unstage a File
```bash
echo "BAD CHANGE" >> file.txt
git add file.txt
git status       # Staged
git restore --staged file.txt
git status       # Back to modified, unstaged
git restore file.txt   # Also undo the change itself
```

### 3. Reset --soft (Undo Commit, Keep Changes Staged)
```bash
git log --oneline    # 3 commits
git reset --soft HEAD~1
git log --oneline    # Only 2 commits
git status           # Changes from "Commit 3" are still staged and ready to recommit
```

### 4. Reset --mixed (Undo Commit, Unstage Changes — DEFAULT)
```bash
git reset HEAD~1
git log --oneline    # Went back to Commit 1
git status           # Changes are unstaged (in working directory)
```

### 5. Reset --hard (Undo Commit AND Discard All Changes) ⚠️
```bash
# Recommit what we had
git add . && git commit -m "Commit 2" && git add . && git commit -m "Commit 3"
git reset --hard HEAD~1
git log --oneline    # Back to 2 commits
cat file.txt         # "line 3" is GONE. Cannot be recovered!
```

### 6. Revert (Safe — Creates an Undo Commit)
Use this when the bad commit is already on a shared remote:
```bash
git log --oneline
BAD_SHA=$(git log --oneline | head -1 | cut -d' ' -f1)
git revert $BAD_SHA
# Editor opens for the revert commit message. Save and close.
git log --oneline   # The original commit is still there + a new "Revert" commit
cat file.txt        # The change from the reverted commit is gone
```

## Key Takeaways
| Command | What it undoes | Destroys data? |
|---------|---------------|----------------|
| `git restore <file>` | Unstaged changes | ✅ Yes |
| `git restore --staged <file>` | Staging only | ❌ No |
| `git reset --soft HEAD~1` | Commit only | ❌ No |
| `git reset --mixed HEAD~1` | Commit + unstage | ❌ No |
| `git reset --hard HEAD~1` | Commit + all changes | ✅ Yes |
| `git revert <sha>` | Add undo commit | ❌ No (safe for shared branches) |
