# GitHub — Remote Repositories, Collaboration, and Workflows

## What is GitHub?
**GitHub** is a cloud-based hosting platform for Git repositories. It adds collaboration features, a web UI, and CI/CD infrastructure on top of Git's core version control. Think of it this way: **Git is the tool, GitHub is the platform**. Alternative platforms exist (GitLab, Bitbucket), but GitHub is the most widely used, hosting over 300 million repositories.

---

## Remote Repositories
A **remote** is a version of your repository hosted on the Internet or a network. In GitHub, this is the repository at `github.com/username/repo-name`.

- **`origin`**: The conventional name given to the default remote when you clone a repository.
- **`upstream`**: The conventional name for the original repository when you've forked someone else's project.

### Key Remote Operations
| Command | Description |
|---------|-------------|
| `git clone <url>` | Download a remote repo and set up `origin` automatically |
| `git remote -v` | List all configured remotes and their URLs |
| `git fetch origin` | Download new commits from remote **without** merging |
| `git pull origin main` | Fetch + merge in one step |
| `git push origin main` | Upload your local commits to the remote |

---

## Forks and Pull Requests (The GitHub Collaboration Flow)
GitHub's primary collaboration mechanism is the **Fork → Branch → Pull Request** workflow:

1. **Fork:** Create your own copy of someone else's repository on GitHub (server-side clone).
2. **Clone:** Download your fork to your local machine.
3. **Branch:** Create a feature branch for your changes (`git checkout -b feature/my-change`).
4. **Commit and Push:** Make changes, commit, and push your branch to your fork.
5. **Pull Request (PR):** On GitHub, open a PR from your fork's branch to the original repo's `main` branch. This is a formal request for your changes to be reviewed and merged.

Within a PR, reviewers can leave comments on specific lines, request changes, approve, or merge the PR.

---

## Issues
GitHub Issues are tickets used to track bugs, feature requests, questions, and tasks. They support labels (e.g., `bug`, `enhancement`), milestones, assignees, and can be linked to specific commits and PRs.

---

## GitHub Actions (CI/CD)
**GitHub Actions** is GitHub's built-in CI/CD pipeline system. You define **Workflows** as YAML files in `.github/workflows/`. Workflows are triggered by events (push, PR open, schedule, manual) and run **Jobs** on virtual machines (Ubuntu, Windows, macOS).

```yaml
# .github/workflows/ci.yml
name: Run Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test
```

---

## Protecting the Main Branch
In GitHub → Repository Settings → Branches → Branch protection rules, you can:
- **Require PR reviews** before merging.
- **Require status checks** (e.g., all CI tests must pass).
- **Prevent force pushes** to `main`.
- **Require signed commits**.

---

## GitHub vs Git — Quick Reference

| Aspect | Git | GitHub |
|--------|-----|--------|
| Type | CLI tool | Web platform |
| Storage | Local `.git` folder | Cloud servers |
| Collaboration | Manual (patches, email) | PRs, Code Review |
| CI/CD | No | GitHub Actions |
| Authentication | SSH keys / HTTPS | OAuth, PATs, SSH |
