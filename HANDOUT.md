# Introduction to Git – Workshop handout

Welcome! In this workshop you'll build a tiny restaurant menu, one text file at a time, and use Git to track every change.
You don't need to know any programming language. If you can edit a text file and type a command in the terminal, you're ready.

**How to use this handout:** we'll do each exercise live on screen. Code along with us, or follow the steps here at your own pace.
Lines starting with `#` in the command boxes are comments. You don't need to type them.

**Golden rule:** when in doubt, run `git status`. It tells you where you are and what Git thinks is going on.

---

## Before we start

1. **Check that Git is installed.** Open a terminal and run:

   ```bash
   git --version
   ```

   You should see something like `git version 2.x.x`. You need version **2.28 or newer**.

   No Git yet? Install it like this:

   - **macOS:** run `git --version` anyway. If Git is missing, macOS offers to install the *Command Line Tools*. Click **Install** and wait a few minutes.
   - **Windows:** download and install **Git for Windows** from [git-scm.com](https://git-scm.com/downloads). The default options are fine. Then use the **Git Bash** app as your terminal today, so all the commands in this handout work.
   - **Linux:** install it with your package manager, for example `sudo apt install git`.
   - **Still stuck?** Pair up with your neighbour for now and raise your hand. We'll help you get set up.

2. **Create a folder for today.** Everything in the workshop happens inside it:

   ```bash
   cd ~                 # start in your home folder
   mkdir git-workshop   # or any name you like, e.g. introduction-to-git
   cd git-workshop
   ```

   Keep this terminal open. If you get lost later, `cd` back into this folder.

**Tip:** some commands, like `git log`, show their output one screen at a time. Press **d** to scroll down, **u** to scroll up and **q** to quit.

---

## Exercise 1: Clone a repository

**Goal:** copy a famous open-source project, history and all, and explore it.

[Express](https://expressjs.com) is one of the most popular web frameworks for Node.js. Its repository is public, so anyone can clone it, including you.

1. **Find it.** Search the web for "expressjs github", or go to [github.com](https://github.com) and search for `expressjs`.
   Make sure you've found the right repository: it should be **`expressjs/express`**.
   Click the green **Code** button to see its address.

2. **Clone it** into your workshop folder, then check that it's there:

   ```bash
   git clone https://github.com/expressjs/express.git
   ls -l                # there's a new express folder
   ```

3. **Look inside:**

   ```bash
   cd express
   ls -la               # the project files, plus the hidden .git folder
   ```

4. **Explore the history:**

   ```bash
   git log --oneline            # every commit, newest first
   git log --reverse --patch    # start from the very first commit and see what changed
   ```

   The very first commit is from **26 June 2009**. You just downloaded over 15 years of history in a few seconds!

5. **See who built it:**

   ```bash
   git shortlog                 # every commit message, grouped by author
   git shortlog -sn             # just the number of commits per person, most first
   ```

6. **Go back to your workshop folder:**

   ```bash
   cd ..
   ```

   **Fun fact:** Express's main branch is called `master`, not `main`. That used to be Git's default name, and many older projects still use it.

✅ **Done when:** you've seen the first commit from 2009, and you're back in your `git-workshop` folder.

---

## Exercise 2: Create your own repository

**Goal:** start a brand-new repository for a restaurant menu.

1. Make sure you're in your `git-workshop` folder (not inside `express`!), then create a folder for the menu and turn it into a repository:

   ```bash
   mkdir my-menu
   cd my-menu
   git init -b main
   ```

   Git replies with `Initialized empty Git repository in .../my-menu/.git/`. The `-b main` part names the first branch `main`.

2. Look around:

   ```bash
   ls -a        # the hidden .git folder is where Git keeps all the history
   git status   # "No commits yet"
   ```

✅ **Done when:** `git status` says `On branch main` and `No commits yet`.

---

## Exercise 3: Tell Git who you are

**Goal:** make sure Git knows your name and email.

Every commit is labelled with a name and email. Git won't let you commit until it knows yours.

1. Make sure you're inside `my-menu`, then check what's already set:

   ```bash
   git config user.name
   git config user.email
   ```

   If both print your details, you're done: skip to the next exercise. If they print nothing, or you want to change them, go on.

2. Choose where to save your name and email:

   - **Just this repository (local).** Nothing else on your computer changes. A good choice on a shared or borrowed computer.

     ```bash
     git config user.name "Your Name"
     git config user.email "you@example.com"
     ```

   - **Every repository on your computer (global).** This is what you'd usually do once on your own laptop.

     ```bash
     git config --global user.name "Your Name"
     git config --global user.email "you@example.com"
     ```

   Local settings win over global ones, so you can still set a different email for a single repository later.

3. Check again:

   ```bash
   git config user.name
   git config user.email
   ```

✅ **Done when:** both commands print your details.

---

## Exercise 4: Make commits

**Goal:** save snapshots of your project and look at its history.

1. Create the menu with its first dish:

   ```bash
   echo "Margherita pizza - 12 €" > menu.txt
   git status
   ```

   `menu.txt` is listed as **untracked**: Git sees the file but isn't tracking it yet.

2. Add the file to the **staging area** (the things that go into the next commit), then check again:

   ```bash
   git add menu.txt
   git status
   ```

   The file is now listed under `Changes to be committed`.

3. Commit it, with a message that says what you did:

   ```bash
   git commit -m "Add pizza to menu"
   ```

4. Add a second dish and see exactly what changed:

   ```bash
   echo "Carbonara pasta - 14 €" >> menu.txt
   git diff
   ```

   Lines starting with `+` were added. (Use `>>` to add a line. A single `>` replaces the whole file!)

5. Commit the change and look at the history:

   ```bash
   git add menu.txt
   git commit -m "Add pasta to menu"
   git log --oneline
   ```

   You now have two commits, each with its own ID.

✅ **Done when:** `git log --oneline` shows two commits and `git status` says `nothing to commit, working tree clean`.

💡 **Good commit messages** start with a verb (`Add`, `Fix`, `Remove`), stay short (about 50 characters) and describe one change. `Add pasta to menu` is good. `stuff` is not.

---

## Exercise 5: Stash

**Goal:** put unfinished work aside for a moment, then bring it back.

Sometimes you're halfway through a change when you need a clean working tree, for example to switch branches or to pull your teammates' changes. `git stash` puts your changes on a shelf until you need them again.

1. Start a change, but don't commit it:

   ```bash
   echo "Lasagne - 15 €" >> menu.txt
   git status          # menu.txt is modified
   ```

2. Put it aside:

   ```bash
   git stash
   git status          # "nothing to commit, working tree clean"
   cat menu.txt        # no lasagne
   git stash list      # your change is waiting on the shelf
   ```

3. Bring it back:

   ```bash
   git stash pop
   git diff            # the lasagne is back
   ```

4. Changed your mind? Stash it again and throw it away for good:

   ```bash
   git stash
   git stash drop
   ```

✅ **Done when:** `git stash list` prints nothing and `git status` says `nothing to commit, working tree clean`.

---

## Exercise 6: Ignore files

**Goal:** tell Git which files it should never track.

Some files don't belong in a repository: passwords and API keys, log files, build output. You list them in a file called `.gitignore`.

1. Create a file with a password and a log file:

   ```bash
   echo "ADMIN_PASSWORD=pizza123" > .env
   echo "Kitchen opened at 9:00" > kitchen.log
   git status          # both are untracked, and git add . would add them!
   ```

2. Create a `.gitignore` that lists them. `*.log` means "every file ending in `.log`":

   ```bash
   echo ".env" > .gitignore
   echo "*.log" >> .gitignore
   git status          # only .gitignore is listed now
   ```

3. Commit the `.gitignore`, so everyone working on the project ignores the same files:

   ```bash
   git add .gitignore
   git commit -m "Add .gitignore"
   ```

✅ **Done when:** `git status` says `nothing to commit, working tree clean`, even though `.env` and `kitchen.log` are still in the folder.

⚠️ **Never commit passwords or API keys.** Once they're in the history, deleting the file later doesn't remove them. `.gitignore` only works for files Git isn't tracking yet.

---

## Exercise 7: Undo changes

**Goal:** throw away a change, unstage a file and fix your last commit.

1. **Throw away a change you haven't staged.** Break the file on purpose, then undo it:

   ```bash
   echo "Oops" >> menu.txt
   git diff                # see the unwanted line
   git restore menu.txt    # go back to the last commit
   cat menu.txt            # the mistake is gone
   ```

2. **Unstage a file.** Sometimes you `git add` something too early. `git restore --staged` takes it out of the staging area, but the change stays in the file:

   ```bash
   echo "Oops" >> menu.txt
   git add menu.txt
   git status                       # "Changes to be committed"
   git restore --staged menu.txt
   git status                       # back to "Changes not staged for commit"
   git restore menu.txt             # now throw the change away too
   ```

3. **Fix the message of your last commit.** Add a dish, but make a typo in the commit message:

   ```bash
   echo "Garlic bread - 5 €" >> menu.txt
   git add menu.txt
   git commit -m "Add garlc bread"
   git log --oneline                # oops: "garlc"
   ```

   `--amend` replaces the last commit with a fixed one:

   ```bash
   git commit --amend -m "Add garlic bread"
   git log --oneline                # fixed, and still four commits
   ```

✅ **Done when:** `git log --oneline` shows `Add garlic bread` at the top and `git status` says `nothing to commit, working tree clean`.

---

## Exercise 8: Undo commits

**Goal:** take back a commit, either safely or by rewriting history.

1. **The safe way: `git revert`.** Change your mind about the garlic bread. `git revert` doesn't delete the commit. It adds a new commit that does the opposite:

   ```bash
   git revert HEAD --no-edit        # HEAD means "the commit you're on now"
   git log --oneline                # a new "Revert ..." commit at the top
   cat menu.txt                     # the garlic bread is gone
   ```

   Because nothing is deleted, this is the safe choice for commits you've already shared with others.

2. **Rewriting history: `git reset`.** Add a dish and commit it:

   ```bash
   echo "Pineapple pizza - 11 €" >> menu.txt
   git add menu.txt
   git commit -m "Add pineapple pizza"
   ```

   Now take that commit back completely. `HEAD~1` means "one commit before HEAD":

   ```bash
   git reset HEAD~1                 # the commit is gone, but the change stays in the file
   git status                       # menu.txt is modified again
   git restore menu.txt             # ...and now the change is gone too
   git log --oneline                # no trace of the pineapple pizza
   ```

   **Only reset commits you haven't shared yet.** If someone else already has the commit, rewriting history causes trouble for them. Use `git revert` instead.

✅ **Done when:** `menu.txt` has just the pizza and the pasta, and `git log --oneline` shows the `Revert "Add garlic bread"` commit at the top.

---

## Exercise 9: Create a branch

**Goal:** work on something new without touching `main`.

1. Create a branch called `desserts` and switch to it:

   ```bash
   git switch -c desserts
   ```

2. Add a dessert and commit it:

   ```bash
   echo "Tiramisu - 6 €" >> menu.txt
   git add menu.txt
   git commit -m "Add tiramisu"
   ```

3. Switch back to `main` and look at the menu:

   ```bash
   git switch main
   cat menu.txt    # no tiramisu here!
   ```

   The dessert only exists on the `desserts` branch. `main` is untouched.

4. List your branches and switch back and forth to see the file change:

   ```bash
   git branch          # the * shows which branch you're on
   git switch desserts
   cat menu.txt        # tiramisu is back
   git switch main
   ```

✅ **Done when:** `git branch` lists both `main` and `desserts`, and you're on `main`.

---

## Bonus exercise: Travel back in time

**Goal:** jump back to the very first commit of Express, then come back to today.

`git switch` doesn't only work with branches. You can switch to any commit, and Git swaps every file in the folder for that commit's snapshot.

1. Go to the Express folder you cloned in exercise 1 and count its files:

   ```bash
   cd ../express
   git ls-files | wc -l           # about 200 files today
   ```

2. Jump to the very first commit, from 2009. `--detach` means "visit this commit, not a branch":

   ```bash
   git switch --detach 9998490
   git ls-files | wc -l           # just 7 files!
   ls                             # the whole project back then
   cat README.rdoc                # the original description
   git status                     # "HEAD detached at 9998490f"
   ```

   "Detached" sounds scary, but it just means you're looking at an old commit instead of a branch. Look around, but don't make commits here.

3. Come back to today, then go back to your menu:

   ```bash
   git switch master              # Express calls its main branch master
   git ls-files | wc -l           # all the files are back
   cd ../my-menu
   ```

✅ **Done when:** you've seen the 2009 version of Express, switched it back to `master`, and you're back in `my-menu`.

---

## Exercise 10: Merge

**Goal:** bring the dessert work into `main`.

1. While you were on `desserts`, work continued on `main`. Make sure you're on `main`, then add a drinks menu there:

   ```bash
   git switch main
   echo "Lemonade - 4 €" > drinks.txt
   git add drinks.txt
   git commit -m "Add drinks menu"
   ```

2. Merge the `desserts` branch into `main`:

   ```bash
   git merge desserts --no-edit
   ```

   `--no-edit` accepts Git's default message for the merge commit, so no text editor opens.

3. Check the result:

   ```bash
   cat menu.txt                    # tiramisu is now on main
   ls                              # drinks.txt is here too
   git log --oneline --graph       # see the two lines of work join up
   ```

✅ **Done when:** `main` has both `drinks.txt` and the tiramisu, and `git log --oneline --graph` shows a merge commit.

---

## Exercise 11: Resolve a merge conflict

**Goal:** see what happens when two branches change the same line, and fix it.

1. Create a `discount` branch and lower the pizza price. Open `menu.txt` in any text editor (for example `code menu.txt` in VS Code) and change `12 €` to `10 €` on the pizza line. Save, then commit:

   ```bash
   git switch -c discount
   # edit menu.txt: pizza costs 10 €
   git add menu.txt
   git commit -m "Discount pizza"
   ```

2. Meanwhile, on `main`, someone raises the price of the same pizza. Switch back and change `12 €` to `13 €` on the pizza line:

   ```bash
   git switch main
   # edit menu.txt: pizza costs 13 €
   git add menu.txt
   git commit -m "Raise pizza price"
   ```

3. Try to merge:

   ```bash
   git merge discount
   ```

   Git stops with `CONFLICT (content): Merge conflict in menu.txt`. Don't panic, this is normal!

4. Open `menu.txt`. Git has marked the conflict:

   ```text
   <<<<<<< HEAD
   Margherita pizza - 13 €
   =======
   Margherita pizza - 10 €
   >>>>>>> discount
   ```

   The part above `=======` is your branch (`main`), the part below is the branch you're merging (`discount`).
   Decide which price to keep (or write a new one), and delete the `<<<<<<<`, `=======` and `>>>>>>>` lines. Save the file.

5. Tell Git the conflict is resolved and finish the merge:

   ```bash
   git add menu.txt
   git commit -m "Merge discount, keep pizza at 10 €"
   git log --oneline --graph
   ```

✅ **Done when:** `menu.txt` has no conflict markers and `git status` says `nothing to commit, working tree clean`.

---

## Exercise 12: Rebase

**Goal:** move your branch on top of the latest `main`, for a straight history with no merge commit.

1. Make sure you're on `main`, then create a `specials` branch and add a specials menu:

   ```bash
   git switch main
   git switch -c specials
   echo "Soup of the day - 7 €" > specials.txt
   git add specials.txt
   git commit -m "Add specials menu"
   ```

2. Meanwhile, someone adds a drink on `main`:

   ```bash
   git switch main
   echo "Espresso - 3 €" >> drinks.txt
   git add drinks.txt
   git commit -m "Add espresso"
   git log --oneline --graph --all     # the two branches have split
   ```

3. Switch to `specials` and rebase it onto `main`:

   ```bash
   git switch specials
   git log --oneline -1    # note the commit ID
   git rebase main
   git log --oneline -1    # same message, new ID!
   git log --oneline --graph
   ```

   Rebase takes your commits and replays them on top of `main`, as if you'd started your branch just now. They become new commits, so they get new IDs.

4. Merge `specials` into `main`:

   ```bash
   git switch main
   git merge specials
   git log --oneline --graph
   ```

   Git says `Fast-forward`: `main` just moves forward to your commit, so there's no merge commit and the history is one straight line.

⚠️ **Only rebase commits you haven't shared yet.** Rebase rewrites history, just like `git reset`. If others already have your commits, use merge instead.

✅ **Done when:** `git log --oneline --graph` on `main` shows `Add specials menu` right above `Add espresso`, with no new merge commit.

---

## Where to go from here

Git is an 80/20 tool: knowing the right 20% gets you 80% of the benefits.

- **Master the basics.** Git has hundreds of commands, but a handful cover most of your daily work: `status`, `add`, `commit`, `log`, `switch`, `merge`.
- **Practise every day.** Use Git on every project, even solo ones. Small commits, clear messages and branches should become a habit.
- **Keep up with AI.** AI tools are changing how we use Git. Follow along, and always read what an agent changed before you commit or merge it.
- **Look into next:** `git push` and `git pull` with GitHub, and `git worktree`.
- **Want to go deeper?** The free [Pro Git book](https://git-scm.com/book) explains everything from the basics up.

---

## Cheat sheet

| Command | What it does |
|---|---|
| `git clone <url>` | Copy an existing repository, with its whole history |
| `git init -b main` | Start a new repository in the current folder |
| `git config user.name` | Show your name. Add `"..."` to set it for this repository, plus `--global` for all of them |
| `git status` | Show what's changed and what's staged |
| `git add <file>` | Stage a file for the next commit |
| `git commit -m "message"` | Save a snapshot with a message |
| `git diff` | Show changes that aren't staged yet |
| `git stash` / `git stash pop` | Put uncommitted changes aside / bring them back |
| `.gitignore` | A file listing files Git should never track, e.g. `.env` or `*.log` |
| `git log --oneline` | Show the history, one commit per line |
| `git restore <file>` | Throw away uncommitted changes to a file |
| `git restore --staged <file>` | Unstage a file, keeping the change |
| `git commit --amend -m "..."` | Replace the last commit, e.g. to fix its message |
| `git revert HEAD` | Undo a commit safely, by adding an opposite commit |
| `git reset HEAD~1` | Remove the last commit but keep its changes (only if not shared yet!) |
| `git switch -c <name>` | Create a new branch and switch to it |
| `git switch <name>` | Switch to an existing branch |
| `git branch` | List branches |
| `git merge <name>` | Merge another branch into the current one |
| `git rebase main` | Replay your branch's commits on top of `main` (only if not shared yet!) |
| `git shortlog -sn` | Count commits per author |

## Help, I'm stuck!

- **The output fills the screen and I can't type.** You're in the pager. Press `q` to quit.
- **A text editor opened in the terminal and I can't get out.** You're probably in Vim. Press `Esc`, type `:wq` and press `Enter` to save and quit.
- **`fatal: not a git repository`.** You're in the wrong folder. Run `cd my-menu` (or wherever your project is).
- **`Please tell me who you are`.** Go back to exercise 3.
- **A rebase stopped halfway.** Run `git rebase --abort` to go back to how things were before.
- **Something else?** Raise your hand. We're happy to help!
