# Lab 01: Initialize a Repo and Make Your First Commit

## Objective
Understand the Git workflow from scratch: initialize a repository, stage files, and create commits.

## Steps

### 1. Configure Git (One-time setup)
```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### 2. Initialize a Repository
```bash
mkdir my-first-repo
cd my-first-repo
git init
ls -la        # See the hidden .git directory created
```

### 3. Inspect the Empty State
```bash
git status    # "On branch main, No commits yet"
```

### 4. Create Files and Stage Them
```bash
echo "# My First Project" > README.md
echo "print('hello world')" > app.py
git status    # Both files are "Untracked" (red)
git add README.md
git status    # README is green (staged), app.py is still red
git add .     # Stage everything
git status    # Both are now green (staged)
```

### 5. Make Your First Commit
```bash
git commit -m "Initial commit: add README and app.py"
git log       # See your commit with SHA, author, date, message
```

### 6. Make a Change and Inspect the Diff
```bash
echo "version = '1.0'" >> app.py
git diff       # Shows unstaged changes (what changed vs last commit)
git add app.py
git diff --staged  # Shows staged changes ready to be committed
git commit -m "Add version number to app.py"
```

### 7. View the Commit Graph
```bash
git log --oneline --graph
```

## Key Takeaways
- `git init` creates the `.git` directory — the entire repo lives there.
- Files go through: **Untracked → Staged → Committed**.
- `git diff` compares working dir to staging; `git diff --staged` compares staging to last commit.
