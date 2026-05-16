# Lab 06: Exploring Git Log and Diff

## Objective
Master the `git log` and `git diff` commands to understand exactly what changed, when, and why.

## Setup
```bash
mkdir log-demo && cd log-demo && git init
for i in 1 2 3 4 5; do
  echo "Feature $i" >> features.txt
  git add . && git commit -m "Add feature $i"
done
```

## Part 1: Git Log Variants
```bash
# Full log (author, date, message, SHA)
git log

# Compact one-liner (SHA + message)
git log --oneline

# Compact with visual branch graph
git log --oneline --graph --all

# Filter by author
git log --oneline --author="Your Name"

# Filter by date
git log --oneline --after="2024-01-01" --before="2024-12-31"

# Filter by keyword in commit message
git log --oneline --grep="feature 3"

# Show commits that affected a specific file
git log --oneline -- features.txt

# Show what actually changed in each commit (patch view)
git log -p

# Show stats (how many lines added/removed)
git log --stat --oneline
```

## Part 2: Git Diff Variants
```bash
# Unstaged changes (working dir vs staging area)
echo "new line" >> features.txt
git diff

# Staged changes (staging area vs last commit)
git add features.txt
git diff --staged

# Compare two branches
git checkout -b compare-branch
echo "different" >> other.txt
git add . && git commit -m "add other.txt"
git checkout main
git diff main..compare-branch

# Diff between two commits (use SHAs from git log)
SHA1=$(git log --oneline | tail -1 | cut -d' ' -f1)
SHA2=$(git log --oneline | head -1 | cut -d' ' -f1)
git diff $SHA1 $SHA2

# Show only filenames changed (not content)
git diff --name-only main..compare-branch
```

## Part 3: Git Show
```bash
# Show a specific commit's full detail
git show HEAD            # Latest commit
git show HEAD~2          # 2 commits ago
git show HEAD:features.txt  # File content at HEAD
```

## Key Takeaways
- `git log --oneline --graph` is your best tool to visualize repository history.
- `git diff` without args = unstaged; `--staged` = what's about to be committed.
- `git log -S "search term"` finds commits that added/removed a specific string (the "pickaxe").
