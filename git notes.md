# Git and GitHub Notes

Revision notes, 23 Oct 2026. Git saves every version of your project on your laptop. GitHub keeps a copy online so it is backed up and visible to recruiters.

## Contents

1. [Basics](#1-basics)
2. [Setup and starting a project](#2-setup-and-starting-a-project)
3. [Daily commands and branches](#3-daily-commands-and-branches)
4. [Undo commands](#4-undo-commands)
5. [Rules and common errors](#5-rules-and-common-errors)
6. [Practice checklist](#6-practice-checklist)

---

## 1. Basics

Git is the tool on your laptop. GitHub is the website where you keep the results.

| | Git | GitHub |
| --- | --- | --- |
| What it is | A tool that records every version of your project | A website that stores Git projects online |
| Works | Offline, on your laptop | Online |
| Example | A diary noting every change you make | Google Drive for that diary |
| Use | Save checkpoints, undo mistakes, try ideas safely | Backup, sharing, showing work to recruiters, teamwork |

### Repository (repo)

A folder with a hidden `.git` notebook inside that remembers its history.

- `git init` creates that notebook, so the folder becomes a repo.
- A repo can hold any number of files and subfolders.
- Git ignores empty folders.

### Key words

| Word | Meaning | Example |
| --- | --- | --- |
| Commit | A saved snapshot, like a checkpoint in a game | "Add TwoSum" |
| Branch | A separate line of work in the same repo | `main`, `practice` |
| Remote | A copy of the repo somewhere else (GitHub) | `origin` is its nickname |
| Push | Send commits from laptop to GitHub | |
| Pull | Download new work from GitHub | |
| Clone | Download a whole GitHub repo for the first time | |
| Merge | Combine one branch into another | |
| Staging area | The "box" where changes wait before a commit | `git add` fills it |

### Core flow (parcel example)

```
Edit file  ->  git add  ->  git commit  ->  git push
              (put it in    (seal it and    (hand it to
               the box)      write a label)  the courier)
```

### Where your work lives

```
Working folder  ->  Staging area  ->  Commits  ->  GitHub
                    (git add)        (git commit)   (git push)
```

Every undo command works on one of these places.

---

## 2. Setup and starting a project

### One time per computer

`--global` means "for every project on this laptop".

```bash
git config --global user.name "Chakri"
git config --global user.email "you@email.com"
git config --list
```

### Once per project, in this order

The folder comes first. The GitHub repo must exist before you push. Git never guesses where to push: the address saved by `git remote add origin` decides.

| Step | What to do | Command |
| --- | --- | --- |
| 1 | Make the folder and go inside | `mkdir dsa-java` then `cd dsa-java` |
| 2 | Turn it into a repo (once, top folder only, never in subfolders) | `git init` |
| 3 | Create files, then save a snapshot | `git add .` then `git commit -m "First commit"` |
| 4 | On github.com: **+**, **New repository**, name `dsa-java`, Public, tick nothing, copy the URL | (in browser) |
| 5 | Save the GitHub address inside this repo | `git remote add origin <url>` |
| 6 | Rename `master` to `main` | `git branch -M main` |
| 7 | First push (`-u` remembers the pairing, so later you only type `git push`) | `git push -u origin main` |

All the commands together:

```bash
mkdir dsa-java
cd dsa-java
git init
git add .
git commit -m "First commit"
git remote add origin https://github.com/yourname/dsa-java.git
git branch -M main
git push -u origin main
```

If the repo already exists on GitHub, use `git clone <url>` instead of steps 1 to 6.

### Several repos

- Each repo is an independent folder with its own history and GitHub address.
- Switch repos with `cd`.
- The branch name in brackets in your prompt shows you are inside a repo.
- Check where a repo will push with `git remote -v`.

| Task | How often |
| --- | --- |
| `git config --global ...` | Once per computer |
| `git init` and `git remote add origin ...` | Once per project |
| `add`, `commit`, `push` | Every time you work |

### Why `git remote add origin`?

You can push with a full URL (`git push <url> main`), but `origin` saves the address under a short nickname. It is like saving a friend's number as "Rahul" instead of dialling 10 digits each time.

- Less typing and fewer typos.
- A repo can have several remotes, for example `origin` for your copy and `upstream` for the original project.
- With `-u`, plain `git push` and `git pull` just work.
- If the address changes, fix it once: `git remote set-url origin <new-url>`.
- `origin` is only the conventional nickname, not a special word.
- With no saved remote, Git says `No configured push destination`.

### Why rename `master` to `main`?

- Git's old default first branch was `master`.
- GitHub changed its default for new repos to `main` in 2020, and most teams and tutorials now use `main`.
- Pushing `master` still works, but your repos become inconsistent, and commands copied from tutorials (`git switch main`, `git push origin main`) can fail.
- `git branch -M main` only changes the label, not your files or commits.
- To avoid renaming every time, set it once:

```bash
git config --global init.defaultBranch main
```

---

## 3. Daily commands and branches

### Daily routine

```bash
cd ~/dsa-java
git pull
# do your work
git status
git add .
git commit -m "Add ValidAnagram"
git push
```

### Everyday commands

| I want to... | Command |
| --- | --- |
| See what changed (safe, use it often) | `git status` |
| Stage one file | `git add file.java` |
| Stage everything that changed | `git add .` |
| Save a snapshot | `git commit -m "message"` |
| Upload to GitHub | `git push` |
| Download the latest from GitHub | `git pull` |
| See history, one line per commit | `git log --oneline` |
| See exactly what changed in files | `git diff` |
| Show where a repo pushes | `git remote -v` |

### Branches

A branch is a separate line of work, like photocopying a story to try a new ending. `main` stays clean while you experiment.

| I want to... | Command |
| --- | --- |
| List branches (`*` is current) | `git branch` |
| Create a branch and move to it | `git switch -c practice` |
| Move to an existing branch | `git switch main` |
| Merge a branch into the one I am on | `git merge practice` |
| First push of a new branch | `git push -u origin practice` |
| Delete a merged branch | `git branch -d practice` |

Branch flow:

```bash
git switch -c practice
# edit files
git add .
git commit -m "Try new solution"
git push -u origin practice
git switch main
git merge practice
git push
```

Branch names cannot contain spaces. Use hyphens, like `notes-origin-and-main`. Spaces are fine only inside a quoted commit message: `git commit -m "Add notes about origin"`.

### Moving around the terminal

| Command | Meaning |
| --- | --- |
| `pwd` | Where am I? |
| `ls` | List files |
| `ls -a` | Show hidden files like `.git` |
| `cd name` | Go into a folder |
| `cd ..` | Up one level |
| `cd ~` | Home folder |

---

## 4. Undo commands

First ask: where is the mistake right now? In a file, in the staging box, in a local commit, or already on GitHub? Run `git status` and `git log --oneline` before and after any undo.

| Scenario | Command | What happens |
| --- | --- | --- |
| Edited a file, want the old version back (not added yet) | `git restore file.java` | Edits are gone for good |
| Added the wrong file to the box | `git restore --staged file.java` | File leaves the box, your edits stay |
| Typo in the last commit message (not pushed) | `git commit --amend -m "new message"` | Only the label changes |
| Forgot a file in the last commit (not pushed) | `git add file`, then `git commit --amend --no-edit` | File joins the last commit |
| Undo last commit, keep everything staged | `git reset --soft HEAD~1` | Commit gone, files stay in the box |
| Undo last commit, keep edits but unstaged | `git reset HEAD~1` | Commit gone, edits stay in files |
| Delete last commit and its edits (dangerous) | `git reset --hard HEAD~1` | Commit and edits both vanish |
| Bad commit already pushed | `git revert HEAD`, then `git push` | A new commit cancels the old one |
| Lost a commit after a hard reset | `git reflog`, then `git reset --hard <id>` | Recovers committed work only |

### Letter example for reset

You sealed a letter and put it in the outbox.

- `--soft` takes it out but keeps it sealed.
- The default (`--mixed`) takes it out and unseals it.
- `--hard` takes it out and bins the letter.

### Rules

- Not pushed yet: use `reset` or `amend`.
- Already pushed: use `revert`, because `reset` rewrites history that GitHub already has.
- `HEAD~1` means one commit before the latest. `HEAD~2` goes back two.
- Treat `--hard` as the dangerous one. Use `--soft` if unsure.
- `reflog` cannot recover edits that were never committed.

---

## 5. Rules and common errors

### Rules

1. One project = one repo = one `git init` = one `.git` folder, at the top folder.
2. Create the GitHub repo before you push.
3. Git pushes to the address saved in the repo your terminal is currently inside.
4. Run `git pull` before you start work, and `git status` whenever you are confused.
5. Commit small and often, with clear messages like "Add TwoSum solution".

### Common errors

| Error | Meaning | Fix |
| --- | --- | --- |
| `fatal: not a git repository` | You are not inside a repo folder | `cd` into the repo |
| `fatal: No configured push destination` | No remote address saved | `git remote add origin <url>` |
| `Repository not found` | GitHub repo missing or wrong URL | Create it on GitHub, check `git remote -v` |
| `nothing to commit, working tree clean` | No changes since the last commit | Nothing wrong; edit a file |
| `fatal: only one reference expected` | Spaces in a branch name | Use hyphens: `git switch -c my-branch` |

---

## 6. Practice checklist

Tick these off as you do them. Doing each command yourself is what makes it stick.

- [ ] Run `git remote -v` in `dsa-java` and read the address it prints
- [ ] Create the empty `dsa-java` repo on GitHub and run `git push -u origin main`
- [ ] Solve Two Sum in Java, then `git add .`, `git commit`, `git push`
- [ ] Make a branch with `git switch -c practice`, commit on it, merge it into `main`
- [ ] Add a `.gitignore` and a README, then push
- [ ] Practice one undo of each kind: `restore`, `restore --staged`, `commit --amend`, `reset --soft HEAD~1`
- [ ] Cause a merge conflict on purpose and fix it