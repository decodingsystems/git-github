# Lab 05: Fork, Clone, and Open a Pull Request

## Objective
Follow the real-world open-source contribution workflow: fork a repo, make a change on a branch, push, and open a Pull Request.

## Prerequisites
- GitHub account.
- SSH configured (see Lab 04).

## Steps

### 1. Fork a Repository
- Go to: `https://github.com/octocat/Spoon-Knife` (a GitHub demo repo for practice).
- Click the **Fork** button (top right). This creates `github.com/YOUR_USERNAME/Spoon-Knife` under your account.

### 2. Clone YOUR Fork
```bash
git clone git@github.com:YOUR_USERNAME/Spoon-Knife.git
cd Spoon-Knife
git remote -v
# origin → your fork (you can push to this)
```

### 3. Add the Original Repo as "Upstream"
```bash
git remote add upstream git@github.com:octocat/Spoon-Knife.git
git remote -v
# origin → your fork
# upstream → the original repo
```

### 4. Create a Feature Branch
```bash
git checkout -b fix/update-readme
```
*Always work on a branch, not directly on main.*

### 5. Make a Change
```bash
echo "<!-- Contribution by YOUR_NAME -->" >> README.md
git add README.md
git commit -m "docs: add contributor comment to README"
```

### 6. Push to YOUR Fork
```bash
git push -u origin fix/update-readme
```

### 7. Open the Pull Request
- Go to `github.com/YOUR_USERNAME/Spoon-Knife`.
- GitHub will show a banner: **"Your branch has recent pushes → Compare & pull request"**. Click it.
- Select:
  - **Base repository:** `octocat/Spoon-Knife` / `main`
  - **Head repository:** `YOUR_USERNAME/Spoon-Knife` / `fix/update-readme`
- Fill in the PR title and description explaining your change.
- Click **Create pull request**.

### 8. Keeping Your Fork Up-to-Date
```bash
git checkout main
git fetch upstream              # Get changes from the original repo
git merge upstream/main         # Integrate them into your local main
git push origin main            # Update your fork on GitHub
```

## Anatomy of a Good PR Description
```markdown
## What does this PR do?
Adds a contributor comment to the README.

## Why?
Improves attribution for contributors.

## Testing
No functional change. Visual inspection only.
```

## Key Takeaways
- **Fork** = server-side copy under your account. **Clone** = local copy.
- `upstream` = original repo. `origin` = your fork. This is a convention everyone uses.
- PRs always go FROM your feature branch → TO the original repo's main branch.
- Sync your fork regularly via `upstream` to avoid large conflicts.
