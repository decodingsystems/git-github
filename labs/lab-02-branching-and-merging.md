# Lab 02: Branching and Merging

## Objective
Create and switch between branches, make changes in isolation, and merge them back to `main`.

## Steps

### 1. Setup
```bash
mkdir branching-demo && cd branching-demo
git init
echo "# App" > README.md
git add . && git commit -m "Initial commit"
```

### 2. Create a Feature Branch
```bash
git branch feature/login           # Create branch (don't switch)
git branch                         # See all branches (* = current)
git checkout feature/login         # Switch to the new branch
# Or in one step:
# git checkout -b feature/login
```

### 3. Add a Feature Commit on the Branch
```bash
echo "def login(): pass" > auth.py
git add auth.py
git commit -m "feat: add login function skeleton"
git log --oneline   # Only see this commit + initial
```

### 4. Switch Back to Main — Feature is Gone
```bash
git checkout main
ls     # auth.py is NOT here — branches are isolated!
git log --oneline   # Only the initial commit visible
```

### 5. Create a Second Branch from Main
```bash
git checkout -b feature/signup
echo "def signup(): pass" > user.py
git add . && git commit -m "feat: add signup function"
```

### 6. Merge Feature/Login into Main (Fast-forward)
```bash
git checkout main
git merge feature/login
# This is a "fast-forward" merge — main just moves forward
git log --oneline   # Initial commit + login commit
ls    # auth.py appears!
```

### 7. Merge Feature/Signup (Creates a Merge Commit)
```bash
git merge --no-ff feature/signup -m "merge: integrate signup feature"
git log --oneline --graph   # See the branch lines and merge commit
ls    # Both auth.py and user.py are here
```

### 8. Clean up Merged Branches
```bash
git branch -d feature/login
git branch -d feature/signup
git branch   # Only main remains
```

## Key Takeaways
- Branches are just lightweight pointers. Creating them is instant.
- **Fast-forward merge:** No divergence, pointer just advances.
- **3-way merge (--no-ff):** Explicit merge commit preserves branch history.
