# Ultimate Git & GitHub Interview Guide

*(Commands + Purpose + Q\&A + Best Practices)*

---

## 1️⃣ Git vs GitHub

| Feature            | Git                                      | GitHub                              |
| ------------------ | ---------------------------------------- | ----------------------------------- |
| **Definition**     | Distributed Version Control System (VCS) | Cloud hosting service for Git repos |
| **Where it works** | Local machine                            | Online                              |
| **Purpose**        | Track & manage code history              | Collaboration, sharing, automation  |
| **Example Tools**  | CLI, Git Bash                            | Website, GitHub Desktop             |

---

## 2️⃣ Git Architecture

```
Working Directory → Staging Area → Local Repository → Remote Repository
```

* **Working Directory** → Code you are editing right now
* **Staging Area (Index)** → Temporary storage for commits
* **Local Repository** → Stored in `.git` folder
* **Remote Repository** → GitHub, GitLab, Bitbucket etc.

---

## 3️⃣ Git Setup

| Command                                            | Purpose                           |
| -------------------------------------------------- | --------------------------------- |
| `git config --global user.name "Your Name"`        | Set username for commits (global) |
| `git config --global user.email "you@example.com"` | Set email for commits (global)    |
| `git config --list`                                | View all configurations           |

---

## 4️⃣ Repository Commands

| Command                       | Purpose                                     |
| ----------------------------- | ------------------------------------------- |
| `git init`                    | Initialize a new Git repo in current folder |
| `git clone <url>`             | Download an existing repo from remote       |
| `git remote -v`               | Show remote repo links                      |
| `git remote add origin <url>` | Link local repo to remote origin            |

---

## 5️⃣ File Tracking & Commit

| Command                    | Purpose                                |
| -------------------------- | -------------------------------------- |
| `git status`               | Show modified/untracked files          |
| `git add file.txt`         | Stage specific file for commit         |
| `git add .`                | Stage all changed files                |
| `git commit -m "Message"`  | Save staged changes with a message     |
| `git commit -am "Message"` | Add + commit tracked files in one step |

---

## 6️⃣ View Changes & History

| Command                            | Purpose                    |
| ---------------------------------- | -------------------------- |
| `git log`                          | Show commit history        |
| `git log --oneline`                | Compact commit history     |
| `git log --graph --decorate --all` | Visual commit history      |
| `git diff`                         | Show unstaged file changes |
| `git diff --staged`                | Show staged file changes   |

---

## 7️⃣ Branching

| Command                     | Purpose                   |
| --------------------------- | ------------------------- |
| `git branch`                | List branches             |
| `git branch new-feature`    | Create new branch         |
| `git checkout new-feature`  | Switch to branch          |
| `git checkout -b feature`   | Create + switch to branch |
| `git merge branch-name`     | Merge branch into current |
| `git branch -d branch-name` | Delete branch (safe)      |
| `git branch -D branch-name` | Force delete branch       |

---

## 8️⃣ Remote Operations

| Command                   | Purpose                          |
| ------------------------- | -------------------------------- |
| `git push -u origin main` | Push local branch to remote      |
| `git push`                | Push changes to remote           |
| `git pull origin main`    | Pull + merge from remote         |
| `git fetch origin`        | Download changes without merging |

---

## 9️⃣ Undoing & Resetting

| Command                         | Purpose                                     |
| ------------------------------- | ------------------------------------------- |
| `git restore file.txt`          | Undo changes in working dir                 |
| `git restore --staged file.txt` | Unstage file                                |
| `git reset --hard HEAD`         | Reset repo to last commit (danger)          |
| `git reset --soft HEAD~1`       | Undo last commit but keep changes staged    |
| `git revert <commit_id>`        | Create new commit to undo a specific commit |

---

## 🔠 Merge Conflicts

| Command                 | Purpose               |
| ----------------------- | --------------------- |
| `git merge branch-name` | Merge branch          |
| *(Manual edit)*         | Fix conflicting files |
| `git add file.txt`      | Mark resolved         |
| `git commit`            | Finalize merge        |

---

## 1️⃣3️⃣ Stash

| Command           | Purpose                              |
| ----------------- | ------------------------------------ |
| `git stash`       | Save uncommitted changes temporarily |
| `git stash list`  | View stashes                         |
| `git stash apply` | Apply stash but keep it saved        |
| `git stash pop`   | Apply + remove stash                 |

---

## 1️⃣4️⃣ Advanced Commands

| Command                        | Purpose                                        |
| ------------------------------ | ---------------------------------------------- |
| `git rebase main`              | Replay commits on top of main (linear history) |
| `git cherry-pick <commit_id>`  | Copy specific commit to current branch         |
| `git tag v1.0`                 | Create lightweight tag                         |
| `git tag -a v1.0 -m "Release"` | Create annotated tag                           |
| `git push origin v1.0`         | Push tag to remote                             |
| `git rebase -i HEAD~3`         | Interactive rebase for squashing commits       |

---

## 1️⃣5️⃣ GitHub Features

* **Pull Request (PR)** → Request to merge code with review
* **Issues** → Track bugs & features
* **Actions** → CI/CD automation
* **Fork** → Copy repo to your account
* **Branch Protection** → Prevent unsafe merges
* **GitHub Pages** → Host static sites

---

## 1️⃣6️⃣ Common Interview Q\&A

**Q:** `git fetch` vs `git pull`?
**A:** Fetch downloads changes but doesn’t merge; Pull = fetch + merge.

**Q:** Merge vs Rebase?
**A:** Merge keeps both branch histories; Rebase makes a linear history.

**Q:** Reset vs Revert vs Restore?
**A:** Reset changes HEAD pointer; Revert makes a commit to undo; Restore resets file contents.

**Q:** How to resolve merge conflict?
**A:** Open file → fix → `git add` → `git commit`.

---

## 1️⃣7️⃣ Production Scenarios

**Rollback Commit**

```bash
git revert <commit_id>   # Safe rollback
```

**Hotfix**

```bash
git checkout main
git pull origin main
git checkout -b hotfix
# fix bug
git commit -m "Hotfix"
git push origin hotfix
# Create PR
```

**Update Feature Branch**

```bash
git checkout feature
git pull origin main
git rebase main
```
