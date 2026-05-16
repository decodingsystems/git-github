# Lab 11: Git Hooks — Automate Quality Checks at Commit Time

## Objective
Use Git hooks to automatically enforce code quality rules (linting, commit message format) without relying on CI pipelines.

## What are Git Hooks?
Git Hooks are scripts inside `.git/hooks/` that Git automatically executes at specific points in the workflow. They're local only (not committed to the repo) unless shared via a tool like Husky.

### Common Hooks
| Hook | When it runs |
|------|-------------|
| `pre-commit` | Before commit is created (linting, formatting) |
| `commit-msg` | After you type a commit message (validate format) |
| `pre-push` | Before pushing to remote (run tests) |
| `post-merge` | After a successful merge (e.g., run `npm install`) |

## Steps

### 1. Setup
```bash
mkdir hooks-demo && cd hooks-demo
git init
echo "console.log('hello')" > app.js
git add . && git commit -m "Initial"
```

### 2. Create a pre-commit Hook (Block Commits with Debug Statements)
```bash
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash

echo "[pre-commit] Checking for debug statements..."

if grep -rn "console.log\|debugger\|TODO: REMOVE" --include="*.js" .; then
    echo "[pre-commit] ❌ ERROR: Found debug statements. Please remove them before committing."
    exit 1    # Non-zero exit = abort the commit
fi

echo "[pre-commit] ✅ No debug statements found."
exit 0
EOF
chmod +x .git/hooks/pre-commit
```

### 3. Test the Hook
```bash
echo "console.log('debug me')" >> app.js
git add app.js
git commit -m "Add debug"
# Hook fires and BLOCKS the commit!
# Output: [pre-commit] ❌ ERROR: Found debug statements...

# Fix it
sed -i '/console.log/d' app.js
git add app.js
git commit -m "Clean code"
# Hook fires and ALLOWS the commit!
```

### 4. Create a commit-msg Hook (Enforce Conventional Commits)
```bash
cat > .git/hooks/commit-msg << 'EOF'
#!/bin/bash
MSG=$(<"$1")
PATTERN="^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,80}"

if ! echo "$MSG" | grep -qE "$PATTERN"; then
    echo "❌ ERROR: Commit message does not follow Conventional Commits format."
    echo "Format: <type>(<scope>): <subject>"
    echo "Types: feat | fix | docs | style | refactor | test | chore"
    echo "Example: feat(auth): add JWT token validation"
    exit 1
fi
exit 0
EOF
chmod +x .git/hooks/commit-msg
```

### 5. Test commit-msg Hook
```bash
echo "change" >> app.js && git add app.js
git commit -m "made a change"     # ❌ Blocked — bad format
git commit -m "feat: made a change"  # ✅ Allowed
```

### 6. Sharing Hooks via Husky (Team Setup)
Hooks in `.git/hooks/` are not committed. For team-wide hooks use Husky (Node.js projects):
```bash
npm install husky --save-dev
npx husky init
# Creates .husky/ folder which IS committed to the repo
# Add pre-commit hook:
echo "npm test" > .husky/pre-commit
```

## Key Takeaways
- Hooks live in `.git/hooks/`. Any non-zero exit code aborts the operation.
- `pre-commit` is the most widely used — runs linters/formatters before every commit.
- `commit-msg` enforces commit conventions (Conventional Commits is the industry standard).
- Use Husky or Lefthook to share hooks with your entire team via version control.
