# Git & GitHub — Complete Beginner Notes

> A practical note for learning Git from the beginning.
>
> The goal of this note is not only to memorize commands, but to understand **what Git is doing, why we use each command, and how Git and GitHub work together**.

---

<!-- ## Table of Contents

1. [What is Git?](#1-what-is-git)
2. [Git vs GitHub](#2-git-vs-github)
3. [Why Do We Need Git?](#3-why-do-we-need-git)
4. [Install Git](#4-install-git)
5. [Check Git Installation](#5-check-git-installation)
6. [Git Configuration](#6-git-configuration)
7. [Git's Basic Idea](#7-gits-basic-idea)
8. [Create a Git Repository](#8-create-a-git-repository)
9. [git status](#9-git-status)
10. [The Three Important Areas](#10-the-three-important-areas)
11. [git add](#11-git-add)
12. [git commit](#12-git-commit)
13. [The Basic Git Workflow](#13-the-basic-git-workflow)
14. [git log](#14-git-log)
15. [git diff](#15-git-diff)
16. [Working with Multiple Files](#16-working-with-multiple-files)
17. [The .gitignore File](#17-the-gitignore-file)
18. [Branches](#18-branches)
19. [Create and Switch Branches](#19-create-and-switch-branches)
20. [Merge](#20-merge)
21. [Merge Conflicts](#21-merge-conflicts)
22. [Remote Repository](#22-remote-repository)
23. [GitHub](#23-github)
24. [git remote](#24-git-remote)
25. [git clone](#25-git-clone)
26. [git push](#26-git-push)
27. [git pull](#27-git-pull)
28. [git fetch](#28-git-fetch)
29. [Clone vs Pull](#29-clone-vs-pull)
30. [Fork](#30-fork)
31. [Pull Request](#31-pull-request)
32. [git restore](#32-git-restore)
33. [git reset](#33-git-reset)
34. [git stash](#34-git-stash)
35. [git tag](#35-git-tag)
36. [GitHub Pages and Deployment](#36-github-pages-and-deployment)
37. [MkDocs + Git Workflow](#37-mkdocs--git-workflow)
38. [A Real Example](#38-a-real-example)
39. [Common Mistakes](#39-common-mistakes)
40. [Useful Commands Cheat Sheet](#40-useful-commands-cheat-sheet)
41. [A Simple Mental Model](#41-a-simple-mental-model)

--- -->

# 1. What is Git?

Git is a **version control system**.

Version control means:

> Git keeps track of changes made to our files over time.

For example, suppose we have:

```text
project/
├── index.html
├── style.css
└── script.js
```

Today everything works.

Tomorrow we change `script.js` and something breaks.

Without Git, we may have to remember what we changed.

With Git, we can see:

- what changed
- when it changed
- who changed it
- previous versions of the project
- which changes belong to which version

So Git is basically a system for **tracking the history of a project**.

---

# 2. Git vs GitHub

This is one of the most important things to understand.

## Git

Git is the actual **version control software** running on our computer.

We use commands like:

```bash
git init
git add
git commit
git branch
git merge
```

Git can work completely without GitHub.

---

## GitHub

GitHub is an online platform where Git repositories can be stored and shared.

So:

```text
Git
↓
works on our computer

GitHub
↓
stores/shares Git repositories online
```

Git is the tool.

GitHub is an online service that works with Git repositories.

---

# 3. Why Do We Need Git?

Imagine we are developing a project.

At different times, we might have:

```text
project-final
project-final-2
project-final-new
project-final-new-real
project-final-new-real-2
```

This becomes difficult to manage.

Git solves this problem.

Instead of making many copies, Git stores the history:

```text
Version 1
   ↓
Version 2
   ↓
Version 3
   ↓
Version 4
```

We can move through the project's history when necessary.

---

# 4. Install Git

First install Git on the computer.

After installation, Git commands become available in the terminal.

On Windows, Git Bash is also commonly installed with Git.

---

# 5. Check Git Installation

Open a terminal and run:

```bash
git --version
```

Example:

```text
git version 2.x.x
```

If a version appears, Git is installed.

---

# 6. Git Configuration

Before making commits, we normally configure our name and email.

```bash
git config --global user.name "Your Name"
```

Example:

```bash
git config --global user.name "Tanim"
```

Set email:

```bash
git config --global user.email "your-email@example.com"
```

Check the configuration:

```bash
git config --global --list
```

### Why do we need this?

Every commit has information about the person who created it.

So Git records:

```text
Author: Tanim
Email: your-email@example.com
```

---

# 7. Git's Basic Idea

The most important Git concept is:

```text
Working Directory
       ↓
   git add
       ↓
Staging Area
       ↓
 git commit
       ↓
Git Repository
```

We should understand this before memorizing commands.

---

# 8. Create a Git Repository

Suppose we have a project:

```text
my-project/
├── index.html
├── style.css
└── script.js
```

Open the terminal inside the project folder.

Run:

```bash
git init
```

Git will create a hidden `.git` directory.

Conceptually:

```text
my-project/
├── .git/
├── index.html
├── style.css
└── script.js
```

The `.git` directory contains Git's internal repository information and history.

### Important

Do not manually edit or delete `.git`.

If `.git` is deleted, that local Git repository's history/configuration is removed from that folder.

---

# 9. git status

This is one of the commands we will use most often.

```bash
git status
```

It tells us the current state of the repository.

For example:

```text
On branch main

Untracked files:
  index.html
  style.css
```

This means Git sees the files, but they are not currently being tracked.

### Very useful habit

When confused, run:

```bash
git status
```

It often tells us what is happening and what Git expects next.

---

# 10. The Three Important Areas

Git can be understood using three main areas.

## 1. Working Directory

This is the actual project files we edit.

Example:

```text
index.html
script.js
style.css
```

---

## 2. Staging Area

This is where we prepare changes that should go into the next commit.

We use:

```bash
git add
```

---

## 3. Repository

This contains committed history.

We use:

```bash
git commit
```

So:

```text
Edit files
   ↓
Working Directory
   ↓
git add
   ↓
Staging Area
   ↓
git commit
   ↓
Repository
```

---

# 11. git add

Suppose we changed `script.js`.

Check:

```bash
git status
```

Then:

```bash
git add script.js
```

Now the change is staged.

Check again:

```bash
git status
```

Git will show that `script.js` is ready to be committed.

---

## Add multiple files

```bash
git add index.html style.css script.js
```

---

## Add everything

```bash
git add .
```

This stages changes in the current directory and its subdirectories.

### Important

`git add .` does **not** mean "save the project permanently".

It only prepares the changes for the next commit.

---

# 12. git commit

After staging the changes:

```bash
git commit -m "Add homepage"
```

A commit is a saved point in the Git history.

For example:

```text
commit 1
"Create project"

commit 2
"Add navigation"

commit 3
"Fix navbar"

commit 4
"Add TypeScript notes"
```

The message should describe what the commit does.

Good:

```bash
git commit -m "Add TypeScript notes"
```

Good:

```bash
git commit -m "Fix navigation links"
```

Less useful:

```bash
git commit -m "update"
```

---

# 13. The Basic Git Workflow

This is the workflow we will use again and again.

### Step 1 — Change files

Edit the project normally.

### Step 2 — Check what changed

```bash
git status
```

### Step 3 — Stage the changes

```bash
git add .
```

### Step 4 — Commit

```bash
git commit -m "Describe the changes"
```

So the basic workflow is:

```text
Edit
 ↓
git status
 ↓
git add .
 ↓
git commit -m "message"
```

If the project is connected to GitHub, there is usually another step:

```text
git push
```

So:

```text
Edit
 ↓
git status
 ↓
git add .
 ↓
git commit
 ↓
git push
 ↓
GitHub
```

---

# 14. git log

To see commit history:

```bash
git log
```

Example:

```text
commit abc123...
Author: Tanim
Date: ...

    Add TypeScript notes

commit def456...
Author: Tanim
Date: ...

    Add JavaScript notes
```

A shorter version:

```bash
git log --oneline
```

Example:

```text
abc1234 Add TypeScript notes
def4567 Add JavaScript notes
123abcd Create website
```

This is much easier to read.

---

# 15. git diff

`git diff` shows changes that have not been staged yet.

Suppose we change:

```text
console.log("Hello")
```

to:

```text
console.log("Hello World")
```

Run:

```bash
git diff
```

Git shows what changed.

This is useful before running:

```bash
git add .
```

because we can inspect our changes first.

---

## Difference between status and diff

```bash
git status
```

tells us **which files changed** and their Git state.

```bash
git diff
```

shows **the actual content changes**.

---

# 16. Working with Multiple Files

Suppose we changed:

```text
index.html
style.css
script.js
```

We can stage all:

```bash
git add .
```

Then:

```bash
git commit -m "Update website"
```

But sometimes we only want some changes.

For example:

```bash
git add index.html
```

Then only `index.html` is staged.

This is useful when changes belong to different tasks.

---

# 17. The .gitignore File

Some files should not be uploaded to Git.

Examples:

```text
node_modules/
.env
dist/
```

We can create:

```text
.gitignore
```

Example:

```gitignore
node_modules/
.env
dist/
```

Now Git will ignore those paths when deciding what untracked files to report.

---

## Why ignore node_modules?

Node projects can contain thousands of installed dependency files.

Instead of uploading the entire `node_modules` folder, we normally keep the dependency information in files such as:

```text
package.json
package-lock.json
```

Then another developer can install dependencies with:

```bash
npm install
```

---

## Why ignore .env?

`.env` files may contain secrets such as:

```text
API_KEY=...
DATABASE_PASSWORD=...
```

We should not accidentally publish secrets to GitHub.

---

# 18. Branches

A branch allows us to work on a separate line of development.

Suppose our main project is:

```text
main
```

We want to add a new feature.

Instead of directly changing `main`, we can create:

```text
feature/navbar
```

Now:

```text
main
  \
   feature/navbar
```

We work on the feature branch.

After the feature is finished, we can merge it back into `main`.

---

# 19. Create and Switch Branches

See current branch:

```bash
git branch
```

Create a branch:

```bash
git branch feature-navbar
```

Switch to it:

```bash
git switch feature-navbar
```

A convenient command creates and switches at the same time:

```bash
git switch -c feature-navbar
```

Now we are working on:

```text
feature-navbar
```

---

## Older command

You may also see:

```bash
git checkout -b feature-navbar
```

This is still widely encountered.

For learning the newer workflow, `git switch` is easier to understand because it is specifically designed for branch switching.

---

# 20. Merge

Suppose we have:

```text
main
feature-navbar
```

We finished the feature.

First switch to `main`:

```bash
git switch main
```

Then merge:

```bash
git merge feature-navbar
```

Conceptually:

```text
feature-navbar
       \
        → main
```

The changes from the feature branch are brought into `main`.

---

# 21. Merge Conflicts

Sometimes Git cannot automatically merge two changes.

Example:

On one branch:

```javascript
const title = "Hello";
```

Another branch changes the same line to:

```javascript
const title = "Hello World";
```

Git may not know which version should win.

This creates a **merge conflict**.

Git may mark the file like:

```text
<<<<<<< HEAD
const title = "Hello";
=======
const title = "Hello World";
>>>>>>> feature-navbar
```

We manually decide which code should remain.

Then:

```bash
git add .
```

and create the merge commit if Git requires one:

```bash
git commit
```

### Important idea

A merge conflict is not necessarily an error in Git.

It means:

> Git needs a human to decide which conflicting change should remain.

---

# 22. Remote Repository

Until now we have mostly talked about Git on our computer.

A **remote repository** is another Git repository, usually hosted somewhere online.

For example:

```text
Local repository
       ↓
   Internet
       ↓
Remote repository
```

GitHub is a common place to host the remote repository.

---

# 23. GitHub

GitHub allows us to:

- store Git repositories online
- collaborate with other developers
- review code
- create pull requests
- manage issues
- publish websites
- keep a backup of our repository

A common setup is:

```text
Your computer
   ↓
Local Git repository
   ↓
GitHub
   ↓
Remote Git repository
```

---

# 24. git remote

To see configured remote repositories:

```bash
git remote -v
```

Example:

```text
origin  https://github.com/username/project.git (fetch)
origin  https://github.com/username/project.git (push)
```

Usually the remote is named:

```text
origin
```

`origin` is just a conventional name.

It is not a special requirement that the remote must be called `origin`.

---

## Add a remote

```bash
git remote add origin https://github.com/username/project.git
```

Check:

```bash
git remote -v
```

---

# 25. git clone

Suppose a project already exists on GitHub.

We want to download the repository to our computer.

Use:

```bash
git clone https://github.com/username/project.git
```

Git will:

- download the repository
- create a local project directory
- bring the Git history
- configure the remote repository

Then:

```bash
cd project
```

---

# 26. git push

`git push` sends local commits to a remote repository.

Example:

```bash
git push
```

For the first push of a new branch, we may use:

```bash
git push -u origin main
```

The `-u` option sets the upstream relationship so future pushes can usually be shorter:

```bash
git push
```

---

# 27. git pull

`git pull` brings changes from the remote repository into the current local branch.

Example:

```bash
git pull
```

Conceptually:

```text
GitHub
  ↓
git pull
  ↓
Your local repository
```

This is useful when the remote repository has changes that your local repository does not have.

---

# 28. git fetch

`git fetch` downloads information about changes from the remote repository without automatically merging those changes into the current branch.

```bash
git fetch
```

Conceptually:

```text
GitHub
  ↓
git fetch
  ↓
Local Git knows about remote changes
```

But your current working branch is not automatically updated by the fetch itself.

This makes `fetch` useful when we want to inspect remote changes before deciding what to do.

---

# 29. Clone vs Pull

These are commonly confused.

## git clone

Used when we **do not have the repository locally yet**.

```bash
git clone URL
```

Think:

> "Give me this repository for the first time."

---

## git pull

Used when we **already have a local copy** and want newer remote changes.

```bash
git pull
```

Think:

> "Update my existing local repository."

---

# 30. Fork

A **fork** is a GitHub feature.

Suppose another developer has a repository and we want our own GitHub copy of it.

We can fork it.

Conceptually:

```text
Other person's repository
          ↓
        Fork
          ↓
Your GitHub repository
```

A fork is especially useful when contributing to projects where we do not have direct write access.

---

# 31. Pull Request

A Pull Request (PR) is a request to merge changes from one branch/repository into another.

Typical workflow:

```text
Fork repository
      ↓
Clone repository
      ↓
Create branch
      ↓
Make changes
      ↓
Commit
      ↓
Push branch
      ↓
Create Pull Request
      ↓
Review
      ↓
Merge
```

Pull Requests are heavily used in collaborative development.

---

# 32. git restore

Sometimes we change a file but decide we do not want those changes.

Suppose:

```text
script.js
```

was modified.

We can restore the working-tree version from the last recorded state:

```bash
git restore script.js
```

### Important

This discards the uncommitted changes in that file.

So be careful.

Do not use it on changes you may still need.

---

## Unstage a file

Suppose we ran:

```bash
git add script.js
```

but we do not want it staged anymore.

Use:

```bash
git restore --staged script.js
```

This removes the file from the staging area but keeps the changes in the working directory.

---

# 33. git reset

`git reset` can move the current branch's history pointer and/or change staging state.

Because it can be destructive, beginners should use it carefully.

A common use is unstaging:

```bash
git reset HEAD script.js
```

This is an older/common form of removing a file from the staging area.

Modern Git documentation often encourages:

```bash
git restore --staged script.js
```

for this specific task.

---

## Resetting a commit

You may encounter:

```bash
git reset --soft HEAD~1
```

This moves the branch back one commit while keeping the changes staged.

Another form:

```bash
git reset --mixed HEAD~1
```

moves the branch back one commit and keeps the changes in the working directory but unstaged.

A dangerous form is:

```bash
git reset --hard HEAD~1
```

This can discard changes.

### Beginner rule

Do not use:

```bash
git reset --hard
```

unless you understand exactly what changes will be removed.

---

# 34. git stash

Sometimes we have unfinished work but need to temporarily switch branches.

For example:

```text
You are working on feature A
        ↓
Something urgent needs to be fixed on main
        ↓
But your current work is not ready to commit
```

We can temporarily store the changes:

```bash
git stash
```

Now the working directory becomes clean.

We can switch branches:

```bash
git switch main
```

After finishing the urgent work, return to the previous branch.

Then restore the stashed changes:

```bash
git stash pop
```

Conceptually:

```text
Unfinished changes
       ↓
   git stash
       ↓
Temporary storage
       ↓
Work somewhere else
       ↓
   git stash pop
       ↓
Changes return
```

---

# 35. git tag

A tag can mark a specific point in Git history.

Tags are commonly used for releases.

Example:

```bash
git tag v1.0.0
```

Then:

```bash
git push origin v1.0.0
```

A project might have:

```text
v1.0.0
v1.1.0
v2.0.0
```

This makes important versions easy to identify.

---

# 36. GitHub Pages and Deployment

GitHub can also host static websites through GitHub Pages.

For example, a repository can contain:

```text
HTML
CSS
JavaScript
```

and GitHub Pages can publish the site.

For a documentation project such as MkDocs, the workflow can be:

```text
Edit Markdown
      ↓
Build documentation
      ↓
Deploy generated site
      ↓
GitHub Pages
      ↓
Website
```

---

# 37. MkDocs + Git Workflow

For our notes website, we have a project like:

```text
My_Notes/
├── docs/
│   ├── python/
│   ├── javascripts/
│   ├── frameworks/
│   │   └── typescript.md
│   └── index.md
│
├── mkdocs.yml
└── ...
```

When we add or edit a note:

### Step 1

Edit the Markdown file.

Example:

```text
docs/frameworks/typescript.md
```

### Step 2

Run the local server:

```bash
mkdocs serve
```

This lets us preview the website locally.

Usually the local address looks like:

```text
http://127.0.0.1:8000/
```

### Step 3

Check the page.

Make sure:

- headings work
- table of contents works
- code blocks look correct
- tabs work
- links work
- navigation works

### Step 4

Stop the server:

```text
Ctrl + C
```

### Step 5

Build the site:

```bash
mkdocs build --strict
```

This helps detect documentation/configuration problems.

### Step 6

Commit the changes:

```bash
git add .
git commit -m "Add TypeScript notes"
```

### Step 7

Push to GitHub:

```bash
git push
```

If the project uses `mkdocs gh-deploy` for GitHub Pages deployment, the deployment command can be:

```bash
mkdocs gh-deploy
```

The exact deployment command depends on how the repository is configured.

---

# 38. A Real Example

Suppose we add:

```text
docs/frameworks/typescript.md
```

and modify:

```text
mkdocs.yml
```

First check:

```bash
git status
```

We may see:

```text
modified: mkdocs.yml
untracked: docs/frameworks/typescript.md
```

Preview:

```bash
mkdocs serve
```

After checking everything:

```bash
mkdocs build --strict
```

Then stage:

```bash
git add .
```

Check:

```bash
git status
```

Commit:

```bash
git commit -m "Add TypeScript notes"
```

Push:

```bash
git push
```

If deployment is done with MkDocs:

```bash
mkdocs gh-deploy
```

Then open the published website.

---

# 39. Common Mistakes

## Mistake 1 — Forgetting to commit

Running:

```bash
git add .
```

does not finish the process.

You still need:

```bash
git commit -m "message"
```

---

## Mistake 2 — Forgetting to push

A commit is normally local.

If you want that commit on GitHub:

```bash
git push
```

So:

```text
git commit
```

and:

```text
git push
```

are not the same thing.

---

## Mistake 3 — Thinking git add uploads files

It does not.

```bash
git add .
```

means:

> Put these changes into the staging area.

It does not mean:

> Upload them to GitHub.

---

## Mistake 4 — Thinking Git and GitHub are the same

They are not.

```text
Git      → version control software
GitHub   → online hosting/collaboration platform
```

---

## Mistake 5 — Pushing secrets

Never intentionally commit things like:

```text
.env
API keys
private credentials
passwords
```

Use `.gitignore` where appropriate, but remember:

> `.gitignore` does not magically remove a secret that has already been committed.

If a secret has been exposed, it should be treated as compromised and rotated/revoked.

---

## Mistake 6 — Using git reset --hard without understanding it

This can discard work.

Always understand what Git is going to remove before using destructive commands.

---

## Mistake 7 — Working directly on main for everything

For personal projects, working on `main` can be completely reasonable.

For larger projects, feature branches are commonly used:

```text
main
 ├── feature-login
 ├── feature-payment
 └── fix-navbar
```

---

# 40. Useful Commands Cheat Sheet

## Installation / information

```bash
git --version
git config --global --list
```

---

## Start repository

```bash
git init
```

---

## Check state

```bash
git status
```

---

## Stage

```bash
git add filename
git add .
```

---

## Commit

```bash
git commit -m "message"
```

---

## History

```bash
git log
git log --oneline
```

---

## Changes

```bash
git diff
```

---

## Branches

```bash
git branch
git switch branch-name
git switch -c new-branch
```

---

## Merge

```bash
git switch main
git merge branch-name
```

---

## Remote

```bash
git remote -v
git remote add origin URL
```

---

## Download repository

```bash
git clone URL
```

---

## Upload commits

```bash
git push
```

---

## Download and integrate remote changes

```bash
git pull
```

---

## Download remote information

```bash
git fetch
```

---

## Restore changes

```bash
git restore filename
git restore --staged filename
```

---

## Temporarily store work

```bash
git stash
git stash pop
```

---

## Tags

```bash
git tag
git tag v1.0.0
```

---

# 41. A Simple Mental Model

If you forget everything else, remember this:

```text
                 YOUR COMPUTER
┌──────────────────────────────────────────┐
│                                          │
│  Working Directory                       │
│       │                                  │
│       │ git add                          │
│       ↓                                  │
│  Staging Area                            │
│       │                                  │
│       │ git commit                       │
│       ↓                                  │
│  Local Git Repository                    │
│       │                                  │
│       │ git push                         │
└───────┼──────────────────────────────────┘
        ↓
     GITHUB
   Remote Repository
```

And when someone else has new changes:

```text
GITHUB
   │
   │ git pull
   ↓
YOUR LOCAL REPOSITORY
```

The core cycle is:

```text
CHANGE
  ↓
STATUS
  ↓
ADD
  ↓
COMMIT
  ↓
PUSH
```

And when you need other people's changes:

```text
PULL
```

---

# Final Notes

### The five commands to learn first

Do not try to memorize every Git command at once.

Start with these:

```bash
git status
git add .
git commit -m "message"
git push
git pull
```

Understand what each one does.

### The most important difference

```text
git add
    ↓
staging

git commit
    ↓
save a version in local Git history

git push
    ↓
send commits to remote/GitHub

git pull
    ↓
bring remote changes into your local branch
```

### A normal daily workflow

```bash
git status

# edit files

git status

git add .

git commit -m "Describe what you changed"

git push
```

If other people are working on the same repository, it is often useful to update your local branch before starting work:

```bash
git pull
```

Then work, commit, and push.

### Remember

Git is not something you need to fear.

At the beginning, it may look like there are hundreds of commands. In normal development, however, you repeatedly use a relatively small set of commands.

First become comfortable with:

```text
status
add
commit
push
pull
branch
switch
merge
clone
```

Once these become familiar, the rest of Git becomes much easier to understand.
