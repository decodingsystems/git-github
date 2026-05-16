# Lab 09: .gitignore — Excluding Files from Git

## Objective
Create a proper `.gitignore` file to prevent build artifacts, secrets, and system files from being tracked.

## Why .gitignore Matters
Without it, you risk accidentally committing:
- `.env` files with database passwords and API keys.
- `node_modules/` (thousands of files, ~100MB).
- `__pycache__/` Python bytecode.
- `.DS_Store` (macOS metadata files).
- Build output folders like `dist/`, `build/`, `target/`.

## Steps

### 1. Setup — Simulate a Real Project
```bash
mkdir gitignore-demo && cd gitignore-demo
git init

# Simulate project structure
mkdir node_modules && echo "pretend dependency" > node_modules/lib.js
touch .env
echo "SECRET_KEY=abc123" > .env
echo "console.log('app')" > app.js
mkdir dist && echo "compiled code" > dist/app.min.js
touch .DS_Store
```

### 2. Check What Git Sees Without .gitignore
```bash
git status
# All files including .env, node_modules/, dist/ are untracked — dangerous!
```

### 3. Create .gitignore
```bash
cat > .gitignore << 'EOF'
# Dependencies
node_modules/

# Build output
dist/
build/

# Environment secrets
.env
.env.*
!.env.example     # Exception: DO track the template

# OS generated
.DS_Store
Thumbs.db

# Python
__pycache__/
*.pyc
*.pyo

# IDE
.vscode/
.idea/
*.swp
EOF
```

### 4. Verify git ignores the right files
```bash
git status
# Only app.js appears! node_modules, .env, dist, .DS_Store are all ignored.

git add .
git commit -m "Initial project setup"
```

### 5. gitignore Patterns Explained
```
*         Matches any string of characters
*.log     Ignore all .log files
!error.log  Exception: DO track error.log
dir/      Ignore entire directory
**/logs   Ignore "logs" directory anywhere in the project tree
doc/*.txt Ignore .txt files only directly in /doc
```

### 6. Ignoring Already-Tracked Files
If you accidentally committed `.env` before adding gitignore, you must untrack it:
```bash
git rm --cached .env
echo ".env" >> .gitignore
git add .gitignore
git commit -m "chore: stop tracking .env"
# .env is now removed from git history going forward (but still exists locally)
```
> Note: To purge it from ALL past history, you need `git filter-branch` or BFG Repo-Cleaner.

### 7. Global .gitignore (System-wide Rules)
```bash
git config --global core.excludesfile ~/.gitignore_global
echo ".DS_Store" >> ~/.gitignore_global
echo "Thumbs.db" >> ~/.gitignore_global
# These are ignored in ALL your repos automatically
```

## Key Takeaways
- `.gitignore` is itself a tracked file — commit it to share rules with your team.
- Secrets like `.env` must NEVER be committed. Add them to `.gitignore` before the first commit.
- Use `git rm --cached` to stop tracking files that are already in the repo.
- GitHub provides [language-specific gitignore templates](https://github.com/github/gitignore).
