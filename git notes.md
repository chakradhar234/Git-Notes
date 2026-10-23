# Git and GitHub Notes

Oct 23, 2026 · @chakradhar

Git saves every version of your project on your laptop; GitHub keeps a copy online so it is backed up and visible to recruiters. These notes cover what we learned, in revision order.

## 1. Basics

Git is the tool on your laptop; GitHub is the website where you keep the results.

|  | Git | GitHub |
| --- | --- | --- |
| What it is | A tool that records every version of your project | A website that stores Git projects online |
| Works | Offline, on your laptop | Online |
| Example | A diary noting every change you make | Google Drive for that diary |
| Use | Save checkpoints, undo mistakes, try ideas safely | Backup, sharing, showing work to recruiters, teamwork |

**Repository (repo):** a folder with a hidden `.git` notebook inside that remembers its history. `git init` creates that notebook, so the folder becomes a repo. A repo can hold any number of files and subfolders, but Git ignores empty folders.

**Key words**

| Word | Meaning | Example |
| --- | --- | --- |
| Commit | A saved snapshot, like a checkpoint in a game | "Add TwoSum" |
| Branch | A separate line of work in the same repo | `main`, `practice` |
| Remote | A copy of the repo somewhere else (GitHub) | `origin` is its nickname |
| Push | Send commits from laptop to GitHub |  |
| Pull | Download new work from GitHub |  |
| Clone | Download a whole GitHub repo for the first time |  |
| Merge | Combine one branch into another |  |
| Staging area | The "box" where changes wait before a commit | `git add` fills it |

**Core flow (parcel example):** edit file, then `git add` (put it in the box), `git commit` (seal it and write a label), `git push` (hand it to the courier).

**Where your work lives:** working folder, then staging area (`git add`), then commits (`git commit`), then GitHub (`git push`). Every undo command works on one of these places.

## 2. Setup and starting a project

**One time per computer** (`--global` means "for every project on this laptop"):

```
git config --global user.name "Chakri"
git config --global user.email "you@email.com"
git config --list
```

**Once per project, in this order.** The folder comes first, the GitHub repo must exist before you push, and Git never guesses where to push: the address saved by `git remote add origin` decides.

1. `mkdir dsa-java`, then `cd dsa-java`
2. `git init` turns the folder into a repo (run it once, in the top folder only, never inside subfolders)
3. Create files, then `git add .` and `git commit -m "First commit"`
4. On github.com: **+**, **New repository**, name `dsa-java`, Public, tick nothing, then copy the URL
5. `git remote add origin <url>` saves the GitHub address inside this repo
6. `git branch -M main` renames `master` to `main`, which GitHub expects
7. `git push -u origin main` sends your commits; `-u` remembers the pairing so later you only type `git push`

If the repo already exists on GitHub, use `git clone <url>` instead of steps 1 to 6.

**Several repos:** each is an independent folder with its own history and GitHub address. Switch repos with `cd`; the branch name in brackets in your prompt shows you are inside one. Check where a repo will push with `git remote -v`.

| Task | How often |
| --- | --- |
| `git config --global ...` | Once per computer |
| `git init` and `git remote add origin ...` | Once per project |
| `add`, `commit`, `push` | Every time you work |

## 3. Daily commands and branches

**Daily routine:** `cd ~/dsa-java`, `git pull`, do your work, `git status`, `git add .`, `git commit -m "..."`, `git push`.

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

**Branches.** A branch is a separate line of work, like photocopying a story to try a new ending. `main` stays clean while you experiment.

| I want to... | Command |
| --- | --- |
| List branches (\* is current) | `git branch` |
| Create a branch and move to it | `git switch -c practice` |
| Move to an existing branch | `git switch main` |
| Merge a branch into the one I am on | `git merge practice` |
| First push of a new branch | `git push -u origin practice` |
| Delete a merged branch | `git branch -d practice` |

Branch flow: `git switch -c practice`, edit, `git add .`, `git commit`, `git push -u origin practice`, `git switch main`, `git merge practice`, `git push`.

**Moving around the terminal:** `pwd` (where am I), `ls` (list files), `ls -a` (show hidden files like `.git`), `cd name` (go into a folder), `cd ..` (up one level), `cd ~` (home folder).

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

**Letter example for reset:** you sealed a letter and put it in the outbox. `--soft` takes it out but keeps it sealed, the default takes it out and unseals it, `--hard` takes it out and bins the letter.

**Rule:** not pushed yet means use `reset` or `amend`. Already pushed means use `revert`, because `reset` rewrites history that GitHub already has. `HEAD~1` means one commit before the latest; `HEAD~2` goes back two.

**Safety:** treat `--hard` as the dangerous one and use `--soft` if unsure. `reflog` cannot recover edits that were never committed.

## 5. Rules and common errors

1. One project = one repo = one `git init` = one `.git` folder, at the top folder.
2. Create the GitHub repo before you push.
3. Git pushes to the address saved in the repo your terminal is currently inside.
4. Run `git pull` before you start work, and `git status` whenever you are confused.
5. Commit small and often, with clear messages like "Add TwoSum solution".

| Error | Meaning | Fix |
| --- | --- | --- |
| `fatal: not a git repository` | You are not inside a repo folder | `cd` into the repo |
| `fatal: No configured push destination` | No remote address saved | `git remote add origin <url>` |
| `Repository not found` | GitHub repo missing or wrong URL | Create it on GitHub, check `git remote -v` |
| `nothing to commit, working tree clean` | No changes since the last commit | Nothing wrong; edit a file |

## 6. Practice checklist

Tick these off as you do them; doing each command yourself is what makes it stick.

- [ ] Run `git remote -v` in `dsa-java` and read the address it prints
- [ ] Create the empty `dsa-java` repo on GitHub and run `git push -u origin main`
- [ ] Solve Two Sum in Java, then `git add .`, `git commit`, `git push`
- [ ] Make a branch with `git switch -c practice`, commit on it, merge it into `main`
- [ ] Add a `.gitignore` and a README, then push
- [ ] Practice one undo of each kind: `restore`, `restore --staged`, `commit --amend`, `reset --soft HEAD~1`
- [ ] Cause a merge conflict on purpose and fix it
