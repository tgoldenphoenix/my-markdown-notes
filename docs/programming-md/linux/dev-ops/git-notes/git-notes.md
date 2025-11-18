# Git & github notes

Check if Git is installed locally: `git --version`

**Getting help**: `git help <verb>`, for example `git help config`  
`git <verb> -h` show a short list of available options

## Terminologies

- Working tree = working directory
- Index = staging area
- HEAD = Commit tree

How to read git synopsis: [stack overflow](https://stackoverflow.com/questions/60906410/how-do-i-read-git-synopsis-documentation)

Git is a **distributed** version control system (DVCS). In comparison with a **centralized** VCS.

## git config

Show git config: `git config --list`  
You can view all of your settings and where they are coming from using:: `git config --list --show-origin`  
The global gitconfig file on Macbook is at: `/opt/homebrew/etc/gitconfig`

- Mới tải Git về thì run these commands:
  - `git config --global user.name "tgoldenphoenix"`
  - `git config --global user.email johndoe@example.com`
- Sau đó generate & add SSH key on local machine & on Github. Phải có cả authentication key & signing key (cùng một key nhưng tạo 2 chức năng).

To set `main` as the default branch name do: `git config --global init.defaultBranch main`  
GitHub changed the default branch name from `master` to `main` in **mid-2020**

Using Visual Studio Code as your default git editor: `git config --global core.editor "code --wait"`

Nếu không config --global thì sẽ lại gặp lỗi này

```bash
Anonimous% git commit -m "add autohotkey for window"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <anhao@Anonimous.localdomain>) not allowed
```

### ssh key config

**Authentication key**: prove your identity (log in) when communicating with Github (push code). It replaces github password.

- Signing key:
  - used to cryptographically sign your commits and tags, proving that you made them and they weren’t tampered with.
  - Không bắt buộc khi push code.

Một ssh generate ra từ một local machine can only be used for one account on github. Nhưng một ssh key generate ra từ một laptop thinkpad có thể add cho một account github & một account nữa trên backlog git.

- account github `anhaoerv` có 1 ssh add vào git (window) think pad
- account github `anhaophamx` có 2 ssh add vào:
  - thinkpad git WSL
  - macbook ở nhà
- Chạy ngon OK!

Nếu không được "Add as Collaborators" trên github thì chỉ có quyền `clone` repo, không được push.

Sau khi add collaborator thì được quyền push.

SSH key is stored in `/Users/anhao/.ssh/`. Trong này chứa cả private & public key pair. Cái file `.pub` là public key còn cái file `id_ed25519` là private key.

With SSH keys, if someone gains access to your computer, the attacker can gain access to every system that uses that key. To add an extra layer of security, you can add a passphrase to your SSH key. To avoid entering the passphrase every time you connect, you can securely save your passphrase in the SSH agent.

When you set up SSH, you will need to generate a new private SSH key and add it to the SSH agent on your local machine. You must also add the public SSH key to your account on GitHub before you use the key to authenticate or sign commits.

When you generate an SSH key, you can add a **passphrase** to further secure the key. Whenever you use the key, you must enter the passphrase. If your key has a passphrase and you don't want to enter the passphrase every time you use the key, you can add your key to the SSH agent. The SSH agent manages your SSH keys and remembers your passphrase.

If you haven't used your SSH key for a year, then GitHub will automatically delete your inactive SSH key as a security precaution.

Generate ssh key with your github email > add to ssh-agen > add to github

### Initializing a new repository

anki

`git init` (automatically create the `master or main` branch). Executing this command will create a new `.git` subdirectory in your current working directory.

- The command `git clone <repo url>`
  - Automatically creates a remote called `origin` for fetch, push, pull
  - Create local **Remote Tracking Branches** to track the corresponding branches on the remote repository (e.g., `origin/main` for the `main` branch on the `origin` remote).
  - Create a local `main` starting at the same place as `origin/main` and configure it to track the corresponding remote branch (e.g., `origin/main`) để khi `pull & push` thuận tiện hơn.

`origin` is the default name for a remote when you run `git clone`. If you run `git clone -o booyah` instead, then you will have `booyah` as your remote name instead of `origin`.

Git SSH URLs follow a template of: `git@HOSTNAME:USERNAME/REPONAME.git`. Host name là Github or bitbucket

## Viewing state: git diff, status & log

- There are 2 _states_ of files:
  - **tracked**: files that were in the last snapshot, as well as any newly staged files (add); they can be unmodified, modified, or staged.
  - **untracked**: everything else

`git status` view **current status** (show file states, merge conflicts status)  
`git status -uall <file/folder>`: Nếu muốn git status show files trong folders (mặc định chỉ show folder) thì [here](https://stackoverflow.com/questions/28222633/git-status-not-showing-contents-of-newly-added-folder)

### git diff

`git diff` so sánh sự khác nhau của  cùng một file nhưng ở các "stage" khác nhau trong git; `git diff` không dùng để so sánh 2 file khác nhau.  
`git diff` does **NOT** compare file A and file B (there are different tool for that). It compare the same file A in different stages: same file on different commits, staged file and the latest commit, same file but on different branch.

Nếu `git status` không có file nào modified thì `git diff` không có output gì hết.

Modify a file and run `git diff`:

```bash
$ echo "this is a diff example" > diff_test.txt

$ git diff
diff --git a/diff_test.txt b/diff_test.txt
index 6b0c6cf..b37e70a 100644
--- a/diff_test.txt
+++ b/diff_test.txt
@@ -1 +1 @@
-this is a git diff test example
+this is a diff example
```

`---` & `+++` do **NOT** means added & remove the code, nó là ký hiệu mà git assign cho từng file. File a là `---`, file b là `+++`.

```bash
# A diff "chunk"
@@ -1 +1 @@
-this is a git diff test example
+this is a diff example
```

- The **chunk header** `@@ -1 +1 @@` means "line number one had changes":
  - `-1` means from the A version file (the minus `-` sign), extracting one line starting at line 1.
  - `+1` means from the B version file (the plus `+` sign), extracting one line starting at line 1.
  - Đừng có hiểu `+ -` theo nghĩa add/remove line.

- `@@ -34,6 +34,8 @@` means:
  - From file a (`-`), starting from line number 34, extract 6 lines.
  - From file b (`+`), start from line 34, extract 8 lines.
  - Vậy suy ra có two more lines added to the file.

Cách đọc output một số tình huống git diff thường gặp

```bash
$ git diff
diff --git a/README.md b/README.md
index 6424794..e29dc2c 100644
--- a/README.md
+++ b/README.md
@@ -7,4 +7,4 @@ main add feature one
 main add feature two
 A add feature one

-A add feature two
+add feature 10/10
```

- `@@ -7,4 +7,4 @@` => Starting at line `7`, before 4 lines, after 4 lines. Two number `4` means some thing was changed but the total number of line stay the same.
- `@@ -5,3 +5,6 @@` means 3 lines was added. Trong dây `+`, `-` chỉ có nghĩa là "after, before".
- `@@  -2,3 +2,4 @@` => add one line

- `git diff` with no parameter compare current working directory vs staging area (thay đổi chưa `add`)
- `git diff --staged` or `--cached` compare staging area (index) vs last commit (final review before `commit`)
- `git diff HEAD` compare working directory vs last commit.
- When a file path is passed to git diff the diff operation will be scoped to the specified file: `git diff HEAD ./path/to/file`. Omitting HEAD in the example above has the same effect.
- Git diff two commit `git diff a498f47 7274899` compare commits. Or you can use the older `..` operator instead of space character such as `git diff a498f47..7274899`. Nên để 2 cái commit id theo đúng thứ tự thời gian `older..newer`. Nếu để commit ngược lại thì sẽ khó hiểu lắm.
- Trong bối cảnh `git diff` thì `..` giống y chang dùng space character. Còn `...` là để view the changes on the branch containing and up to the second commit, starting at a common ancestor of both commit.

A common workflow might look like:

1. Make changes to multiple files
2. Run `git diff` to review all changes
3. Use `git add <file>` selectively to stage logical groups of changes
4. Run `git diff --staged` to verify what's about to be committed
5. Commit the staged changes with `git commit -m "Your message"`
6. Repeat for other logical groupings of changes

`git diff <hash>` diff với một hash thay vì compare with the last commit. Có thể dùng với tags.

`git diff --stat 1.0`: dùng với tag `1.0`, `--stat` hiển thị number of lines was changed. Git assumes you want to compare against the HEAD if you don’t give it a second revision.

### git range-diff

`git range-diff` is available from git 2.19

`git range-diff` is used to compare two ranges of commits (compared to `git diff` which compares the state at two different commits directly). It's best to think of git range-diff as performing a `diff` of two `git-diff`s—because that's literally how it works behind the scenes!

In theory, this makes it possible to compare a stack of commits prior to **rebasing** with the stack of commits after rebasing and to show the differences between them. If the rebase was simply rearranging and squashing commits then you would expect the diffs to be identical, and the diff of diffs would show that.

On the other hand, if you had to handle merge conflicts as part of the rebase, or if you rebased onto a different commit, then you might expect there to be changes, and these would be shown by git range-diff

[đọc & take note khi có thời gian](https://andrewlock.net/verifiying-tricky-git-rebases-with-range-diffs/)

### `git log` - Viewing the commit history

`git log` the most recent commits show up first. Each commit bao gồm: unique hash, pointer to its parent (except the first commit) & commit info (time, user email/name)  
`git log --oneline`  
`git log -n 8 --oneline` or shorter `git log -8` show only last 8 lines, ko show hết  
`git log -n 8 --oneline origin/main` default là show `log` của `HEAD`.  
By default, `git log` only show the commit history reachable from `HEAD` (below the branch you are standing on). To show commit history for the desired branch, run `git log origin/main`.  
To show all of the branches, add `--all` to your `git log` command.  
Git graph on the terminal: `git log --all --decorate --oneline --graph`. Tùy version có thể omit `--decorate` vì nó default.

```bash
$ git log --oneline
a9d6782 (HEAD -> main, origin/main, origin/HEAD) fetch demo
f4769cf C2
98542a4 Initial commit
```

`git log -p -2`: The `-p, --patch` option shows the `diff` introduced in each commit. You can also limit the number of log entries displayed, such as using `-2` to show only the last two entries.
The `--stat` shows some abbreviated stats for each commit.

`git log <hash>`

`git log --since="5 hours"` look at commits only from the last five hours  
`git log --before="5 hours" -1` skip the last five hours and view only those commits older than that
`--since="24 hours"`, `--since="1 minute"`, `--before="2008-10.01"`

`--since`, `--until`: time-limiting options (useful)

`--author`, `--grep` filter options (useful)

`-S` takes a string and shows only those commits that changed the number of occurrences of that string

### Ancestry References

If you place a `^` (caret) **at the end** of a reference, Git resolves it to mean the parent of that commit. Example: `HEAD^`.

You can also specify a number after the `^` to identify which parent you want; for example, `d921970^2` means “the second parent of d921970.” This syntax is useful only for merge commits, which have more than one parent — the first parent of a merge commit is from the branch you were on when you merged (frequently master), while the second parent of a merge commit is from the branch that was merged (say, topic).

The other main ancestry specification is the `~` (tilde). This also refers to the first parent, so `HEAD~` and `HEAD^` are equivalent. The difference becomes apparent when you specify a number. `HEAD~2` means “the first parent of the first parent,” or “the grandparent” — it traverses the first parents the number of times you specify.

Git use zero-based indexing.

You can also use multiple carets: 18f822e∧∧ is two revisions prior to 18f822e, and so on.

`~N`: The tilde and a number operator subtracts N from the commit name. Using the last examples, `18f822e~1` is the revision prior to 18f822e, and `18f822e~2` is two revisions prior to 18f822e.

### Commit range selection

The double-dot syntax `..` asks Git to resolve a range of commits that are reachable from one commit but aren’t reachable from another.

`git log master..experiment` all commits reachable from experiment that aren’t reachable from `master` = what is in your experiment branch that hasn’t yet been merged into your `master` branch. Khi log ra cũng theo thứ tự newest on top, oldest bottom.

`experiment..main` all commits in master that aren’t in experiment = shows you everything in master not reachable from experiment.

See what you’re about to push to a remote: `git log origin/master..HEAD`  
This command shows you any commits in your current branch that aren’t in the master branch on your origin remote.  
You can also leave off one side of the syntax to have Git assume `HEAD`. For example, you can get the same results as in the previous example by typing `git log origin/master..` — Git substitutes `HEAD` if one side is missing.

The triple-dot syntax `...` specifies all the commits that are reachable by either of two references but not by both of them (lloạii commit chung ra).

`git log master...experiment` to see what is in master or experiment but not any common references.

Trong bối cảnh của lệnh `git diff` thì `..` và `...` có ý nghĩa khác.

You can also pass ranges of commits in by separating two revisions with `git log oldest-revision..newest-revision`. You should always specify the oldest revision first so Git can understand what you want. If you don’t do that, Git won’t complain; it just won’t show you anything:

```bash
prompt> git log 18f822e..0bb3dfb
commit 0bb3dfb752fa3c890ffc781fd6bd5dc5d34cd3be
Author: Travis Swicegood <development@domain51.com>
Date: Sat Oct 4 11:06:47 2008 -0500
add link to twitter
```

At first glance, that output looks wrong. We told it to show us the log from revision 18f822e to revision 0bb3dfb. The first time I used a com-mand similar to this, I thought it would include 18f822e, but Git doesn’t. Instead, Git interprets that range to mean every commit after 18f822e to 5ef8.

`git log 18f822e..HEAD`  
You can drop the final HEAD from that range because Git assumes HEAD is what you mean if you leave the last value blank: `git log 18f822e..`

With ranges, you can swap out the commit name for a tag name too. This is useful for seeing what has changed since a particular tag and for looking at the revision history between two tags.

```bash
prompt> git log --pretty=format:"%h %s" 1.0..HEAD
0bb3dfb add link to twitter
18f822e add contact page
217a88e add the skeleton of an about page
9a23464 rename to more appropriate name
6f1bf6f Change biography link and add contact link
4333289 add in a bio link
```

### reflog

One of the things Git does in the background while you’re working away is keep a reference log (reflog) — a log of where your HEAD and branch references have been for the last few months.  
Every time your branch tip is updated for any reason (`checkout, reset, merge`), Git stores that information for you in this temporary history. You can use your reflog data to refer to older commits as well.

What happens when you accidentally delete a branch using `git branch -D` or a `git rebase -i` goes wrong?1 This is where Git’s reflog comes in.

The reflog keeps track of when a branch changes. Viewing it, you can find the commit you need to check out to restore a branch.

`git reflog` == `git reflog show HEAD`. This will output the HEAD reflog (current branch). You should see output similar to:

```bash
eff544f HEAD@{0}: commit: migrate existing content
bf871fd HEAD@{1}: commit: Add Git Reflog outline
9a4491f HEAD@{2}: checkout: moving from main to git_reflog
9a4491f HEAD@{3}: checkout: moving from Git_Config to main
39b159a HEAD@{4}: commit: expand on git context 
9b3aa71 HEAD@{5}: commit: more color clarification
f34388b HEAD@{6}: commit: expand on color support 
9962aed HEAD@{7}: commit: a git editor -> the Git editor
```

The syntax to access a git ref is `name@{qualifier}`. In addition to HEAD refs, other branches, tags, remotes, and the Git stash can be referenced as well.

`git gc`, which we talked about in Section 9.1, Compacting Repository History, on page 123, will cause some older reflog entries to expire. For example, the commits we just restored are normally deleted after thirty days. This can be changed by changing the gc.reflogExpireUnreachable config setting. Likewise, normal reflog entries are expired after ninety days, unless you change gc.reflogExpire to something else. That is to keep the reflog from getting too large.
You’ll rarely need to use the reflog, but when you do, it can be a life-saver. Rewriting history can be dangerous, and git reflog is a safety net to help keep you safe from yourself.

### git show

show one commit (using hash) & its diff

### git blame

`git blame hello.html` prefixes every line with the commit name, committer, and timestamp. Dùng `git blame` khi muốn narrow xuống chỉ còn một block of code trong một file.

`git blame -L 12,13 hello.html` => The `-L` option tells Git to display only a certain set of line ranges. It takes single parameter—a <start>,<end>.

The ending number doesn’t have to be a specific number. You can spec-ify it as +N or -N to specify a range.

- `git blame -L 12,+2 hello.html`
- `git blame -L 12,-2 hello.html

## Add & Commit

`git add *` does not add deleted files. Chỉ nên dùng `git add .` or `git add -A`.

`git add` is also used to mark merge conflicts as resolved.

- `git commit` launches your shell editor. It is also used to **conclude merges** (nên dùng `merge --continue`).
- `git commit -v` puts the diff of your change in the shell editor.
- `git commit -m <message>` không mở shell editor.
- `git commit -a` automatically stage all _tracked files_ before doing the commit (skip `git add`). To add & commit in one command, run: `git commit -am "message here"`.

When you run git commit, Git creates a new commit object and moves the branch that `HEAD` points to up to it.

`git add -i` interactive add. Có thể add patch mode.

`git add -p` go straight to patch mode, skip interactive. In patch mode, có thể stage only a portion of a file chứ không stage hết entire file.

Không bắt buộc phải stage toàn bộ changes rồi mới được commit. Có thể select only what you want to stage.

How to write atomic commit messages:

one commit for one change. 10 bugs -> 10 commits  
present tense, imperative; give order to codebase; no case

### Managing files

`git rm <filepath>` remove the file from index & working directory, example is if you add your file with passwords.

`git rm file.txt`

`git rm --cached README` _keep the file in your working tree_ but remove it from your staging area.

Lỡ add, push lên github một folder rồi nhưng sau đó lại muốn bỏ thì sau khi cho vào `.gitignore` rồi phải run `git rm` nhé

`git mv file_from file_to` **rename** a file in Git.

### Git ignore

Những thông tin phải `.gitignore` không được push lên remote repositories:

- API keys
- password
- AWS key, open AI keys can get you bankrupt

You can google `gitignore generator` for templates for different kind of project.

## Undoing Things

### Amend & force push

Remember, anything that is committed in Git can almost always be recovered. Even commits that were on branches that were deleted or commits that were overwritten with an --amend commit can be recovered (see Data Recovery for data recovery). However, anything you lose that was never committed is likely never to be seen again.

`git commit --amend` replace (amend) the latest commit without creating an entirely new commit in the history. This command will open the editor with the latest commit message. You can then change the commit message and push it.

- You can change commit message, add & change staged files
- You end up with a single commit — the second commit replaces the results of the first.
- Whenever you are doing amend Git will basically rewrite the entire commit and generates a **new hash** for it. This is very important to understand.

`git push -f` force push (`--force`); used after `commit -amend` to overwrite the old commit on the remote repo.

`--force` overwrites a remote branch with your local branch.

`--force-with-lease` is a safer option that will not overwrite any work on the remote branch if more commits were added to the remote branch (by another team-member or coworker or what have you). It ensures you do not overwrite someone elses work by force pushing.

It is always advisable to use amend when you haven’t pushed the changes to remote or if you are pretty confident that no other developers have started using those changes or no others have pushed any new changes to the current branch.

In the case you’ve pushed the commit to GitHub, there is a big difference if there are multiple people working on the same branch or if you are the only person working on the branch.

Let’s say that there are three people working on the branch main. If you commit your code to main and then someone else uses your code, you should not amend your commit. The only way to ensure that no one else uses your code prior to making an amend to your commit is to not amend your public commit.

However, if you’re the only person working on your branch, then it is safe to amend your commit.

Problems start occurring when the commit is already pushed to GitHub but I want to edit it.

`git restore <file>` to discard changes in modified files and revert them back to the latest commit snapshot version (un-modified a file). :bangbang: You will lose all your local changes to that file (`git checkout` is an old version of this).  
You can check with the checkout command that how files look like but you can only restore the file to the last commit only. You cannot go further.

`git restore --staged <file>` to unstage file (`git reset` is an old version of this). Or `git rm --cache <file>` to unstage

Step 01: First, add some content in README.md. After that, add, commit & push to remote repo.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -am "I will change this commit later"
[main 19c419e] I will change this commit later
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git push origin main
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 372 bytes | 372.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:anhaoerv/demo-git.git
   f52c25b..19c419e  main -> main
```

Step 02: Let's edit the content inside `README.md`, run `git add` then `git commit --amend`.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git add .

$ git commit --amend
```

Git will open `vim` to let you edit the commit message. After saving, we have overwitten the last commit with new content.

```bash
[main 8edb82c] I will change this commit later (already changed). OK!
 Date: Thu Oct 9 08:46:45 2025 +0700
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git log --oneline
8edb82c (HEAD -> main) I will change this commit later (already changed). OK!
```

But now, when we push to `origin`, we get rejected. We need to do a force push.

```bash
$ git push origin main
To github.com:anhaoerv/demo-git.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:anhaoerv/demo-git.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

$ git push -f
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 399 bytes | 399.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:anhaoerv/demo-git.git
 + 19c419e...8edb82c main -> main (forced update)

$ git log --oneline
8edb82c (HEAD -> main, origin/main, origin/HEAD) I will change this commit later (already changed). OK!
```

Our amended commit is now pushed to Github (notice the hash `8edb82c` is matching).

### Reset

Think of Git as content manager of three different trees (collection of files not the Tree Data Structure). Git will compare between these three trees & log information to the user.

1. HEAD: Last commit's snapshot, next parent
2. Index: proposed next commit's snapshot (the "staging area")
3. **Working Directory (or Working tree)**: the "sandbox" where you can try changes out before committing them to your staging area (index) and then to history

- Your "repository" is not a tree, it is the commit history stored inside the `.git/` directory:
  - You have your **local repository** and...
  - The **upstream repository** or the public repository thường ở trên Github.

Git will look at the `index` when you run `git commit`.

- "Changes not staged for commit" => working directory differrent from Index
- "Changes to be committed" => HEAD & Index differ

Step 1: **Move HEAD** (--soft, undid commits): The first thing reset will do is move what HEAD points to. This isn’t the same as changing HEAD itself (which is what `checkout` does); reset moves the branch that HEAD is pointing to. This means if HEAD is set to the `master` branch (i.e. you’re currently on the master branch), running `git reset 9e5e6a4` will start by making master point to 9e5e6a4.  
No matter what form of reset with a commit you invoke, this is the first thing it will always try to do. With `reset --soft`, it will simply stop there.

Step 2: Updating the Index (--mixed, default, unstage everything): The next thing reset will do is to update the index with the contents of whatever snapshot HEAD now points to.

It still undid your last commit, but also unstaged everything. You rolled back to before you ran all your git add and git commit commands.

- **Step 3**: Updating the Working Directory (`--hard`):
  - The third thing that reset will do is to make the working directory **look like the index**. If you use the `--hard` option, it will continue to this stage.
  - You undid your last commit, the git add and git commit commands, and **all the work you did in your working directory**.
  - This flag (--hard) is the only way to make the reset command dangerous, and one of the very few cases where Git will actually destroy data. Any other invocation of reset can be pretty easily undone, but the --hard option cannot, since it forcibly overwrites files in the working directory

`git reset --hard HEAD^` remove the last commit.

- The basics:
  - `--soft` update where a branch point to, undid commits.
  - `--mixed`, default: `--soft` + unstage everything
  - `--hard`: Updating the Working Directory (be careful when use)

Let's try each of them on the command line!

**Step 1**: Add a new file `new_file` with some content in it & change the content of existing `file.txt`. After that, run `git status`.

```bash
$ echo 'new file content' > new_file
$ git add new_file
$ echo 'test git reset' >> file.txt

$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new_file

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file.txt
```

**Step 2.1**: run a **hard** reset.

```bash
$ git log --oneline
9cda0ca (HEAD -> main, feature-branch) add feature
5c7893c update in main
a9d6782 (origin/main, origin/HEAD) fetch demo
f4769cf C2
98542a4 Initial commit

$ git reset --hard
HEAD is now at 9cda0ca add feature

$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

A hard reset deletes all changes in working directory & delete the staging area (permanently lose `new_file` & all changes in `file.txt`)

**Step 2.2**: run a **mixed** reset.

Repeat the set-up commands in **Step 1**. After that, run a `--mixed` reset (default without specifying).

```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new_file

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file.txt

$ git reset --mixed
Unstaged changes after reset:
M       file.txt

$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new_file

no changes added to commit (use "git add" and/or "git commit -a")
```

`new_file` is now **untracked** => mixed reset remove the staging area

**Step 2.3**: run a **soft** reset.

Repeat the set-up commands in **Step 1**. After that, run a `--soft` reset.

```bash
$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new_file

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file.txt

$ git reset --soft

$ git status
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   new_file

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file.txt
```

In this case, `git reset --soft` seems to do nothing for us! This is because, by default, `git reset` is invoked with `HEAD` as the target commit. Let's try soft reset with `HEAD~` (the commit before HEAD).

```bash
$ git log --oneline
9cda0ca (HEAD -> main, feature-branch) add feature
5c7893c update in main
a9d6782 (origin/main, origin/HEAD) fetch demo
f4769cf C2
98542a4 Initial commit

$ git reset --soft HEAD~

anhao@Anonimous MINGW64 ~/Desktop/demo-git (main)
$ git log --oneline
5c7893c (HEAD -> main) update in main
a9d6782 (origin/main, origin/HEAD) fetch demo
f4769cf C2
98542a4 Initial commit
```

`main` now points to commit `5c7893c` & we lose the commit `9cda0ca`.

git reset takes a commit name as its parameter. It defaults to HEAD if you don’t provide one.
You can use the ∧ and ~ commit name modifiers to specify a revision. HEAD^ would reset two commits, while `540ecb7~3` would reset to three commits before 540ecb7.

git reset updates the repository and stages the changes for you to com-mit. This is useful when you notice an error in your previous commit and want to fix it.
Add --soft when you want to stage all the previous commits but not commit them. This gives you a chance to modify the previous commit by adding to or taking away from it.
The final option is --hard, and it should be used with care. It removes the commit from your repository and from your working tree. It’s the equivalent of a delete button on your repository with no “undo.”

Demo using `git reset --soft` to rewrite commit history

```bash
anhao@Anonimous MINGW64 ~/desktop/backlog-new (test-regex)
$ git log --all --decorate --oneline --graph
* adb08c2 (HEAD -> test-regex, origin/test-regex) end of day commit
* 0433e70 regex working ok
* 68504d2 dang test thu regex
*   1de439a (origin/main, origin/HEAD, main) Merge pull request #39 VN_PRO-805 into main
|\
| * ee01528 (origin/VN_PRO-805, VN_PRO-805) VN_PRO-805 コメントとシグネチャ、仕
様を一致させる - Làm cho comment, signature và spec thống nhất với nhau
|/
*   076c31e Merge pull request #38 VN_PRO-803-transaction into main

$ git reset --soft HEAD~3

anhao@Anonimous MINGW64 ~/desktop/backlog-new (test-regex)
$ git log --all --decorate --oneline --graph -8
* adb08c2 (origin/test-regex) end of day commit
* 0433e70 regex working ok
* 68504d2 dang test thu regex
*   1de439a (HEAD -> test-regex, origin/main, origin/HEAD, main) Merge pull request #39 VN_PRO-805 into main
|\
| * ee01528 (origin/VN_PRO-805, VN_PRO-805) VN_PRO-805 コメントとシグネチャ、仕
様を一致させる - Làm cho comment, signature và spec thống nhất với nhau
|/
*   076c31e Merge pull request #38 VN_PRO-803-transaction into main

git add .
git commit

$ git log --all --decorate --oneline --graph -8
* e565a08 (HEAD -> test-regex) VN_PRO-802 関数内部で現在時を参照しない - Không tham chiếu thời gian hiện tại bên trong hàm
| * adb08c2 (origin/test-regex) end of day commit
| * 0433e70 regex working ok
| * 68504d2 dang test thu regex
|/
*   1de439a (origin/main, origin/HEAD, main) Merge pull request #39 VN_PRO-805 into main
|\
| * ee01528 (origin/VN_PRO-805, VN_PRO-805) VN_PRO-805 コメントとシグネチャ、仕様を一致させる - Làm cho comment, signature và spec thống nhất với nhau
|/
*   076c31e Merge pull request #38 VN_PRO-803-transaction into main
```

sau đó có thể pull `main` về, rebase  rồi push lại lên `origin`.

### Demo reset repo về commit đầu tiên

Chạy các câu lệnh sau

```bash
$ git log --oneline
93950ee (HEAD -> main, origin/main, origin/HEAD) update README
49b0394 update README
325d909 update README
50b00fc update README
cde5881 update README all vietnamese
21126aa remove image from README
696d983 test README use image link on backlog instead of local
1d3df0e refactor script.js, update README
52aa0e5 test image on remote git
be3b275 update README vietnamese
a512a6c hide apiKey
f27c0b9 first commit

anhao@Anonimous MINGW64 ~/desktop/backlog-new (main)
$ git reset f27c0b9
Unstaged changes after reset:
M       README.md
M       script.js

$ git log --oneline
f27c0b9 (HEAD -> main) first commit
```

Xóa 2 file `script.js` và `README.md` sau đó chạy lệnh.

```bash
$ git status
On branch main
Your branch is behind 'origin/main' by 11 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    README.md
        deleted:    script.js

no changes added to commit (use "git add" and/or "git commit -a")

anhao@Anonimous MINGW64 ~/desktop/backlog-new (main)
$ git add .

$ git commit --amend -m "reset repository state" --allow-empty
[main d710cfb] reset repository state
 Date: Fri Oct 10 16:04:47 2025 +0700

$ git push --force origin main
Enumerating objects: 2, done.
Counting objects: 100% (2/2), done.
Writing objects: 100% (2/2), 186 bytes | 186.00 KiB/s, done.
Total 2 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To ever-rise.git.backlog.jp:/VN_PRO/backlog-wiki-page-kankopi-button.git
 + 93950ee...d710cfb main -> main (forced update)
```

Repository trên backlog hiện tại chỉ còn duy nhất một  commit. Tất cả commit trước kia đã được xóa.

Repo sau khi hiện tại là empty không chứa file nào cả.

### Git revert

`git revert` takes a specified commit, invert its changes and appends a new commit with the resulting inverse content. The specified commits are **NOT** removed from history. `HEAD` sẽ nhảy lên cái inverse commit mới tạo ra.

`git revert` undo only one specified commit each time it is run.

`git revert HEAD` revert the last commit.

Let's demo on the terminal. First, add 3 commits.

```bash
$ echo "add feature revert 01" >> README.md
$ git commit -am "add feature revert 01"
[main e4e4f47] add feature revert 01
 1 file changed, 1 insertion(+)

$ echo "\nadd feature revert 02" >> README.md
$ git commit -am "add feature revert 02"
[main b2dd9a8] add feature revert 02
 1 file changed, 1 insertion(+)

$ echo "\nadd feature revert 03" >> README.md
$ git commit -am "add feature revert 03"
warning: in the working copy of 'README.md', LF will be replaced by CRLF the next time Git touches it
[main 178ebd6] add feature revert 03
 1 file changed, 1 insertion(+)
```

The content of `README.md` is now as following.

```text
add feature revert 01
add feature revert 02
add feature revert 03
```

When we revert the last commit (feature 03), we got a new commit & the content of `README.md` is updated.

```bash
$ git log --oneline
178ebd6 (HEAD -> main) add feature revert 03
b2dd9a8 add feature revert 02
e4e4f47 add feature revert 01
60b0a10 another commit

$ git revert HEAD
[main 087c9db] Revert "add feature revert 03"
 1 file changed, 1 deletion(-)

$ git log --oneline
087c9db (HEAD -> main) Revert "add feature revert 03"
178ebd6 add feature revert 03
b2dd9a8 add feature revert 02
e4e4f47 add feature revert 01
60b0a10 another commit
```

The content of `README.md` is now updated. Notice that the feature 03 is gone!

```txt
add feature revert 01
add feature revert 02
```

Amend the last commit with `git commit -C HEAD -a --amend` => The option `-C` tells Git to use the log message from the commit specified—in this case HEAD, but it could have been any valid commit name as well—instead of explicitly providing a new message. You use the parameter in its lowercase form, -c, to tell Git to launch the editor with the message already filled in so you tweak it before finalizing the commit.

Amending a commit should be done only when you are working with the last commit. If you made a mistake and need to correct it after other commits have been made, you want to use `git revert`.

Normally Git commits the reversal immediately, but you can add the -n parameter to tell Git not to commit. This is useful when you need to revert multiple commits. Just run multiple git revert commands with the -n parameter, and Git stages all the changes and waits for you to commit them.

You must provide it with a commit name so it knows what to revert. For example, if you wanted to revert the commit 540ecb7 and HEAD, use the following. Always revert backward—the most recent first. That makes sure you don’t have any unnecessary conflicts to work through when reverting multiple commits.

```bash
prompt> git revert -n HEAD
Finished one revert.

prompt> git revert -n 540ecb7
Removed copy.txt
Finished one revert.

prompt> git commit -m "revert 45eaf98 and 540ecb7"
Created commit 2b3c1de: revert 45eaf98 and 540ecb7
2 files changed, 0 insertions(+), 10 deletions(-)
delete mode 100644 copy.txt
```

---

Mình không thể `revert` & `--amend` at the same time. Và không cần phải làm như vậy.

Revert 2 cái commit, thêm thay đổi & tạo duy nhất một commit mới

```bash
# checkout a new branch from main
# undo the changes introduced in these three commit
$ git revert --no-commit D
$ git revert --no-commit C
$ git revert --no-commit B
# add some changes
$ git commit -m "the commit message for all of them"
```

Khi rever nên chọn commit hash là cái mà sẽ được merge into main, nếu chọn cái merge commit, do nó có 2 parents nên nó sẽ bắt mình thêm option nhứt đầu.

## Git branching

A Git repository is a collection of **objects** and **references**:

- The main **objects** in a Git repository are commits objects, but other objects include **blobs** and **trees**.
- The most **important** references in Git are branches. **HEAD** is another important type of reference.

[^1]: A **branch** in Git is simply a lightweight _movable pointer_ to one of the **commit objects**. A branch is not even an object. The default branch name in Git is `master/main`. Every time you commit, the branch you re currently on (pointed to by `HEAD`, usually the `master/main` branch) automatically moves forward to the last commit you made.

A branch in Git is actually a simple file that contains the 40 character SHA-1 checksum of the commit it points to, branches are cheap to create and destroy. Creating a new branch is as quick and simple as writing 41 bytes to a file (40 characters and a newline). The actual big files are the blobs, tree, commit objects. Branches are small in size.  
Git branch = pointer = references (stored in `.git/refs`)

`HEAD` is a special pointer to the local branch you’re currently on. The purpose of HEAD is to keep track of the current point in a Git repo. In other words, HEAD answers the question, “Where am I right now?”  
`HEAD`'s only difference to a branch is that it does **not** point to a commit object. Instead, `HEAD` points to a branch.  
`HEAD` is stored in `.git/HEAD` not in `.git/refs/heads` which stores your branches addresses including `main`.

[Git Detached Head: What This Means and How to Recover](https://www.cloudbees.com/blog/git-detached-head)

For instance, when you use the log command, how does Git know which commit it should start displaying results from? `HEAD` provides the answer.  
When you create a new commit, its parent is indicated by where `HEAD` currently points to.

When you change branches, HEAD is updated to point to the branch you’ve switched to.

The normal state is when `HEAD` points to a branch.  
In the **detached HEAD state**, `HEAD` is pointing directly to a commit instead of a branch (branches always point to the latest commit of its "branch").  
Detaching HEAD allow you to not only take a look at the commits in the past, but also change it. In this state, you can make experimental changes, effectively creating an alternate history.

Scenario 02: I’ve Made Experimental Changes and I Want to Discard Them: You’ve entered the detached HEAD state and made a few commits. The experiment went nowhere, and you’ll no longer work on it. What do you do? You just do the same as in the previous scenario: go back to your original branch. The changes you made while in the alternate timeline won’t have any impact on your current branch.

Scenario 03: I’ve Made Experimental Changes and I Want to Keep Them: If you want to keep changes made with a detached HEAD, just create a new branch and switch to it. You can create it right after arriving at a detached HEAD or after creating one or more commits. The result is the same. The only restriction is that you should do it before returning to your normal branch.

`git branch <branch name>` creates a **new branch** which points to the same commit you’re currently on (`HEAD` point to it). Note that this command does not switch to the newly created branch như `git checkout -b`.  
Now there are two branches point to the same commit object.

`git branch new_branch master` create `new_branch` from `master` instead of the current branch. `checkout -b` cũng giống như vậy.  
`git branch <new_branch> <tag_name>` tạo branch từ tag (thường là để fix lỗi sau khi release).

- `git branch` list **local** branches. The current local branch will be marked with an asterisk (the branch that `HEAD` points to).
- To see all **remote branch** names, run `git branch -r`
- To see all local and remote branches, run `git branch -a`
- `git branch -v` see the last commit on each branch.
- `git branch --merged` see which branches are already merged into the branch you’re on. Branches on this list without the `*` in front of them are generally fine to delete.
- `git branch --no-merged` see all the branches that contain work you haven’t yet merged in

Switch to an existing branch `git checkout <branch name>`. This effectively moves `HEAD` to point to `<branch name>` branch and reverts the files in your working directory back to the snapshot that branch points to.  
`git checkout -b <new branch name>` create a new branch and switch to that new branch in just one command. Giống `-c` của git switch.

`git checkout <HASH-f9f4f1c>` will put you in detach HEAD. You get to the detached HEAD state by checking out a commit directly instead of a branch name.

`git checkout -b [<new-branch>] [<start-point>]`

Delete branch: `git branch -d <branch name>`. You delete branch after merging successfully.

Delete all branch except `main`: `git branch | grep -v "main" | xargs git branch -D`  
This command lists all branches, filters out the main branch, and deletes the rest.

To delete local branches that have already been merged into the current branch (commonly the main branch): `git branch --merged | grep -v "\*" | xargs git branch -d`  
This command skips any branches that contain unmerged changes.

Before cleaning remote branches, synchronize your branch list with: `git fetch --prune`  
This updates your local copy of the remote branch list and removes any references to branches that have been deleted remotely.

Delete local remote-tracking branches: `git remote prune origin`  
This will remove all references to the remote branch that you may have locally. These references are called "remote tracking branches".

delete remote-tracking branches

```bash
$ git branch -dr origin/test-regex
Deleted remote-tracking branch origin/test-regex (was adb08c2).
```

delete the branch from the remote repository: `git push origin --delete <branch-name>`

go two commits prior to this one: `git checkout HEAD~2`. If you are currently on commit 04, it will move you to commit 01. This command also put you in detached HEAD. [read more](https://stackoverflow.com/questions/45924056/why-cannot-the-tilde-sign-be-part-of-a-valid-git-branch-name)

If you’ve reached the detached HEAD state by **accident**—that is to say, you didn’t mean to check out a commit—going back is easy. Just check out the branch you were in before: `git checkout/switch <branch-name>` for example `git checkout/switch main`. You can do this because `main` is stored inside `.git/refs/heads`  
Or you can run `git reflog` which move your `HEAD` back to where you were previously.

From Git version 2.23 onwards you can use `git switch` instead of `git checkout` to:

- Switch to an existing branch: `git switch testing-branch`.
- Create a new branch and switch to it: `git switch -c new-branch`. The `-c` flag stands for `create`, or you can also use the full flag: `--create`.
- Return to your previously checked out branch: `git switch -`.

**Tips:**

- Khi tạo project for learning purpose, nên tạo 1 tag/branch ở initial commit sau này mình checkout về lại đó.
- Always commit before switching branch

Khi switch branch phải có clean working state (no un-commited changes). Trước khi switch branch phải commit hết. Nếu không thì phải stashing & commit amending.

`git branch -m <old_name> <new_name>` re-name an existing branch.

`git branch --move bad-branch-name corrected-branch-name` This replaces your bad-branch-name with corrected-branch-name, but this change is only local for now. To let others see the corrected branch on the remote, push it `git push --set-upstream origin corrected-branch-name`

`git branch --all` List both remote-tracking branches and local branches

`git push origin --delete bad-branch-name` delete bad-branch-name remote branch

`git branch --move master main` Rename your local master branch into main. To let others see the new main branch, you need to push it to the remote. This makes the renamed branch available on the remote: `git push --set-upstream origin main`
`git push origin --delete master` delete the master branch on remote

### prune

```bash
$ git log --oneline
e1f685b (HEAD -> main, origin/main, origin/HEAD) Merge pull request #19 VN_PRO-787-use-const into main
804c9ce (origin/VN_PRO-787-use-const, VN_PRO-787-use-const) VN_PRO-787 再代入しない変
数を const で書き換える
6e503c4 Merge pull request #17 VN_PRO-782-anonymous-handleclick into main
6e8f1be (origin/VN_PRO-782-anonymous-handleclick, VN_PRO-782-anonymous-handleclick) VN_PRO-782 [Kankopi Button] Remove the handleClick function and rewrite it simply as an anonymous function.
24187bd Merge pull request #15 VN_PRO-781 into main
521dcab VN_PRO-781 Sửa cách trình bày tiêu đề trong file Readme - Readmeファイル内の書き方を変更します。
4cbde82 Merge pull request #16 VN_PRO-780-refactor-handleclick into main
c18e493 (origin/VN_PRO-780-refactor-handleclick, VN_PRO-780-refactor-handleclick) VN_PRO-780 [Kankopi Button] Refactor method handleClick() cho nút Copy
906945a Merge pull request #14 VN_PRO-770 into main
4302b66 Dịch Readme sang tiếng Nhật

$ git branch | grep -v "main" | xargs git branch -D
Deleted branch VN_PRO-780-refactor-handleclick (was c18e493).
Deleted branch VN_PRO-782-anonymous-handleclick (was 6e8f1be).
Deleted branch VN_PRO-787-use-const (was 804c9ce).

anhao@Anonimous MINGW64 ~/desktop/backlog-new (main)
$ git remote prune origin
Pruning origin
URL: ever-rise@ever-rise.git.backlog.jp:/VN_PRO/backlog-wiki-page-kankopi-button.git
 * [pruned] origin/VN_PRO-780-refactor-handleclick
 * [pruned] origin/VN_PRO-782-anonymous-handleclick
 * [pruned] origin/VN_PRO-787-use-const

$ git log --oneline
e1f685b (HEAD -> main, origin/main, origin/HEAD) Merge pull request #19 VN_PRO-787-use-const into main
804c9ce VN_PRO-787 再代入しない変数を const で書き換える
6e503c4 Merge pull request #17 VN_PRO-782-anonymous-handleclick into main
6e8f1be VN_PRO-782 [Kankopi Button] Remove the handleClick function and rewrite it simply as an anonymous function.
24187bd Merge pull request #15 VN_PRO-781 into main
521dcab VN_PRO-781 Sửa cách trình bày tiêu đề trong file Readme - Readmeファイル内の書き方を変更します。
4cbde82 Merge pull request #16 VN_PRO-780-refactor-handleclick into main
c18e493 VN_PRO-780 [Kankopi Button] Refactor method handleClick() cho nút Copy
906945a Merge pull request #14 VN_PRO-770 into main
4302b66 Dịch Readme sang tiếng Nhật
```

### Merging branches

Khi muốn bring changes from one branch (`main`) to your personal branch cũng gọi là merge.

Có các trường hợp merge như sau:

1. **Fast-forward merge**: create a branch and work on it để nếu có gì hỏng thì return back to `main`. Git just move the branch pointer forward no new commit (snapshot) created. Khi merge will have no conflict.
2. **Three-way merge**: using the two snapshots pointed to by the branch tips and the common ancestor of the two. Sẽ có conflict nếu 2 branches tips changed the same file. The changes do not necessarily need to the on the same line. It only need to be on the same file, to create merge conflict. Git creates a new commit (snapshot) called **merge commit**. This merge commit points to two parent while normal commit only has one parent.
3. Rebase

Nếu có merge conflict thì git không tạo merge commit mà pause để user resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run `git status`. Git auto add **conflict-resolution markers** to the files that have conflicts, so you can open them manually and resolve those conflicts.

Phân biệt git diff marker & git conflict markers.

Keep whatever you want, remove markers & save. You can manually remove the markers `<<<<<` & `=====` or some editor provide buttons that you can just click.  
Sau khi resolve merge conflict manually in your editor, run `git add` on each file to mark it as resolved. Staging the file marks it as resolved in Git. Run `git status` again to verify that all conflicts have been resolved. Sau đó `git commit -m "resolve merge conflict"`.

Merge 2 branches làm 1 và fastforward đều dùng chung 1 command: Check out the branch you wish to **merge into** (usually `main/master`) and then run the `git merge [branch-name]` command. Think "I'm bringing somthing into me so I want to stand on the final destination".  
Khi fast-forward merge cái branch mình cần merge sẽ đi trước `main` nhưng mình vẫn sẽ checkout về `main`.  
Vì merge branch sẽ tạo ra 1 commit mới nên nó cũng cần có message giống như khi mình commit. Fast-forward thì nó không hỏi mà lấy luôn commit của lastest snapshot.

Ví dụ merge hotfix into main: `git merge hotfix-branch` when standing on `main` branch even though `main` is behind `hotfix-branch`.

`git mergetool` fires up an appropriate visual merge tool and walks you through the conflicts

How to read conflict marker:

```html
<<<<<<< HEAD:about.html
<li>Javascript</li>
=======
<li>EMCAScript</li>
>>>>>>> about2:about.html
```

Any code that is preceded by “<<<<<<<” is the code in your current branch, and any code suffixed with “>>>>>>>” is from the other branch.

### Rebase

In Git, there are two main ways to integrate changes from one branch into another: the merge and the rebase.  
Very often you will want to bring changes from `main` to my personal branch. Merge main into our personal branch allows you to see the latest update. Khi đó thường dùng rebase thay vì merge.

rebase can be used as a clean up tool (clean up commits)

Sau khi rebase xong kể cả không có conflict vẫn phải check lại code.

With the rebase command, you can take all the changes that were committed on one branch and re-apply them on a different branch.

You would check out the `experiment` branch, and then rebase it onto the `master` by running:

```bash
git checkout experiment
git rebase master
```

Nếu merge thì checkout qua `master` rồi merge `feature` into `master`. Checkout to the final branch.

Khi rebase nếu có conflict thì cũng resolve giống như merge bình thường. Nhớ phải làm đúng như nó kêu. Phải resolve conflict manually, rồi `add` rồi run `git rebase --continue`

git rebase `their vs --our`: `our` thực chất là `main` chứ không phải nhánh của mình => dễ bị nhầm

```bash
anhao@Anonimous MINGW64 ~/desktop/demo-git (anhao)
$ git log --oneline
b6be74d (HEAD -> anhao) anhao add 03
d40d7ba anhao add 02
3a19186 (origin/main, origin/HEAD, main) Merge pull request #7 from anhaoerv/anhao

anhao@Anonimous MINGW64 ~/desktop/demo-git (anhao)
$ git log --oneline
b6be74d (HEAD -> anhao) anhao add 03
d40d7ba anhao add 02
3a19186 (origin/main, origin/HEAD, main) Merge pull request #7 from anhaoerv/anhao

$ git checkout anhao
Switched to branch 'anhao'

anhao@Anonimous MINGW64 ~/desktop/demo-git (anhao)
$ git rebase main
Successfully rebased and updated refs/heads/anhao.

anhao@Anonimous MINGW64 ~/desktop/demo-git (anhao)
$ git log --oneline
b239e35 (HEAD -> anhao) anhao add 03
e3eeaa0 anhao add 02
2807118 (main) main add 02
324eb0d main add 01
3a19186 (origin/main, origin/HEAD) Merge pull request #7 from anhaoerv/anhao
```

- `feature` có 2 commit khác `main` thì sau khi rebase:
  - `main` không đổi
  - `feature` vẫn có 2 commit nhưng on the tip of `main`
  - `feature` không có thêm merge commit
  - `feature` không bị mất commit, chỉ bị "rabase"

`git rabase` có conflict khi sửa cùng một dòng. Sửa cùng một file nhưng khác dòng rabase không có conflict.

rebase rồi `push --force` lên để làm đẹp nhánh

```bash
anhao@Anonimous MINGW64 ~/desktop/backlog-new (VN_PRO-785-const-var-first)
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

anhao@Anonimous MINGW64 ~/desktop/backlog-new (main)
$ git pull
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (4/4), 1.03 KiB | 65.00 KiB/s, done.
From ever-rise.git.backlog.jp:/VN_PRO/backlog-wiki-page-kankopi-button
   9d36d53..1244eaa  main       -> origin/main
Updating 9d36d53..1244eaa
Fast-forward
 README.md | 5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)

anhao@Anonimous MINGW64 ~/desktop/backlog-new (main)
$ git checkout -
Switched to branch 'VN_PRO-785-const-var-first'

anhao@Anonimous MINGW64 ~/desktop/backlog-new (VN_PRO-785-const-var-first)
$ git rebase main
Successfully rebased and updated refs/heads/VN_PRO-785-const-var-first.

anhao@Anonimous MINGW64 ~/desktop/backlog-new (VN_PRO-785-const-var-first)
$ git push --force origin HEAD
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 531 bytes | 531.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
To ever-rise.git.backlog.jp:/VN_PRO/backlog-wiki-page-kankopi-button.git
 + 0dc60b2...b685f99 HEAD -> VN_PRO-785-const-var-first (forced update)
```

### Rebase vs merge

- When rebase `A` on-to `main` => `main` will **NOT** move. Only `HEAD->A` move (on to the tip of `main`).
- When merge `A` in-to `main` => `HEAD->main` will move forward pointing to the newly created **merge commit**. `A` will **NOT** move.

- When you need to incorporate changes from `main` into your feature branch, rebase your `feature` onto `main` to avoid unnecessary merge commits.
- Admin will merge your submitted feature branch into `main`.

Rebase cannot replace merge but merge can replace rebase.

This is the starting point: two branches `A & B` branching out from `main`.

```bash
$ git log --oneline --graph --decorate --all
* 253841a (B) B adds new feature
| * c93f63e (A) A add new feature
|/
* fd85cd3 (HEAD -> main) starting point
* a877dab another commit
```

In this snippet, I merged `B` into `main`, then rebased `A` onto `main`.

```bash
$ git log --oneline --graph --decorate --all
* a877dab (HEAD -> A) A add new feature
* 35ac0bd (main, B) B add new feature
* ae58796 starting over the demo
```

In this snippet, I also merged `B` into `main` (fast-forwarding); but now I will merge `A` into `main.

```bash
$ git log --oneline --graph --decorate --all
*   b119f81 (HEAD -> main) Merge branch 'A' into 'main'
|\
| * c93f63e (A) A add new feature
* | 253841a (B) B adds new feature
|/
* fd85cd3 starting point
* a877dab another commit
```

You are working on `feature` branch, your team update `main`. This results in a forked history.

Now, let’s say that the new commits in main are relevant to the feature that you’re working on. To incorporate the new commits into your feature branch, you have two options: merging or rebasing.

to merge changes from `main` into feature branch

```bash
git checkout feature
git merge main
# Or, you can condense this to a one-liner
git merge feature main
```

This creates a new “merge commit” in the `feature` time line that has two parents (`feature & main`).

`main` is not changed in any way. The `feature` branch will have an extraneous merge commit every time you need to incorporate upstream changes. If main is very active, this can pollute your feature branch’s history quite a bit.

As an alternative to merging, you can rebase the feature branch onto main branch using the following commands:

```bash
git checkout feature
git rebase main
```

This moves the entire feature branch to begin on the tip of the main branch. But `main` is still not changed in any way.

### How Git calculate commit hashes

Each commit in Git has a unique SHA-1 hash, which is calculated based on its content, metadata (author, timestamp, commit message), and crucially, the hash of its parent commit(s). When you rebase, the parent commit of your feature branch commits changes (they are now based on the target branch's history). This change in the parent reference, even if the content of the commit itself is identical, results in a new, different SHA-1 hash for each rebased commit.

These hashes is a unique identifier for each commit and acts as a "fingerprint" for the state of the repository at that specific point in time.

For a regular commit, the hash is based on its preceding commit. For a merge commit, it includes the hashes of all parent commits involved in the merge.

### What if your rebase `main` into `feature`?

Nếu rebase `main` onto `feature`, the rebase moves all of the commits in main onto the tip of feature. The problem is that this only happened in your repository. All of the other developers are still working with the original main. Since rebasing results in brand new commits, Git will think that your main branch’s history has diverged from everybody else’s.

The only way to synchronize the two main branches is to merge them back together, resulting in an extra merge commit and two sets of commits that contain the same changes (the original ones, and the ones from your rebased branch). Needless to say, this is a very confusing situation.

So, before you run git rebase, always ask yourself, “Is anyone else looking at this branch?” If the answer is yes, take your hands off the keyboard and start thinking about a non-destructive way to make your changes (e.g., the git revert command). Otherwise, you’re safe to re-write history as much as you like.

When you rebase, Git essentially takes the commits from your feature branch, "removes" them temporarily, and then reapplies them one by one on top of a new base commit (typically the latest commit of the target branch, like main).  
Instead of simply moving the existing commits, Git effectively creates new commits that represent the same changes but are now linked to the new base. The old commits still exist in the Git repository for a period (until garbage collection), but they are no longer directly accessible through the branch pointer.  
In essence, rebase doesn't just move commits; it reconstructs them with a new lineage, leading to the creation of new commits with different hashes. This is why rebasing a branch that has already been pushed to a remote and shared with others can be problematic, as it changes the history that others might have already based their work on.

Notice the commit hashes before rebasing:

```bash
$ git log --all --decorate --oneline --graph
* 0f441e1 (HEAD -> main) main add feature two
* abfa932 main add feature one
| * a4d08f6 (A) A add feature two
| * f41e65e A add feature one
|/
* 93fc0ad starting point
* b119f81 another commit
```

After rebase A into `main`, the two commit of `A` is moved onto the tip of `main` and their hash is changed too. The commit name & content is the same but the hash is changed.

```bash
$ git log --all --decorate --oneline --graph
* 60b0a10 (HEAD -> A) A add feature two
* 738e081 A add feature one
* 0f441e1 (main) main add feature two
* abfa932 main add feature one
* 93fc0ad starting point
*   b119f81 (origin/main, origin/HEAD) Merge branch 'A' into 'main'
```

Không được thay đổi `main` mà không có review.

### Interactive rebasing

Interactive rebasing gives you the opportunity to alter commits as they are moved to the new branch. This is even more powerful than an automated rebase, since it offers complete control over the branch’s commit history. Typically, this is used to clean up a messy history before merging a feature branch into main. You can do editing, deleting, and squashing.

To tell Git where to start the interactive rebase, use the SHA-1 or index of the commit that immediately precedes the commit you want to modify.

Interactive rebasing will create new SHA-1’s therefore it is best to use interactive rebasing on commits you have not pushed to a remote branch.

To begin an interactive rebasing session, pass the i option to the git rebase command:

```bash
git checkout feature
git rebase -i main
```

This will open a text editor listing all of the commits that are about to be moved:

```bash
pick 33d5b7a Message for commit #1
pick 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3
```

In git log the most recent commit is on top. In interactive rebasing view, the most recent commit is on bottom (reverse order).

the following command begins an interactive rebase of only the last 3 commits: `git checkout feature git rebase -i HEAD~3`

Git use zero-based indexing, so `HEAD~1` the last commit & `HEAD~0` is where you are standing on.

Make lots of small commits and tidy them up later using interactive rebase. Use `git push origin --force-with-lease`.

Normally when I'm rewriting history I use git rebase -i in combination with git reset HEAD~. This lets me squash commits together, pause to split them apart, reorder them, or remove them entirely. This is used to modify your pull request to make it easier to review.

[read & note khi có thời gian](https://hackernoon.com/beginners-guide-to-interactive-rebasing-346a3f9c3a6d)

Đọc thêm về cách "squash" commit bằng interactive rebase

re-order commit => run `rebase -i` > change the order of the commits lines

squashing commits => `rebase -` > change `pick` into `squash`

khi `rebase -i` thì order ngược lại với `git log`

Breaking One Commit into multiple commits

### Squashing Commits

Squashed commits take the history of one branch and compress— or “squash”—it into one commit on top of another branch.

Git takes all the history of one branch and compresses it into one commit in the other branch.

Example: You have two commits in your `contact` branch. You can squash these two commits back into the `master` branch as one commit.

```bash
$ git checkout master
Switched to branch "master"

$ git merge --squash contact
Updating 217a88e..2f30ccd
Fast forward
Squash commit -- not updating HEAD
contact.html | 19 +++++++++++++++++++
1 files changed, 19 insertions(+), 0 deletions(-)
create mode 100644 contact.html
```

The `--squash` option tells `git merge` to take all the commits from the other branch and squash them into one commit.  
Now both of your commits from contact have been applied to your work-ing tree and are staged for a commit, but they have not been committed.

All that’s left now is for you to commit the change like any other commit: `git commit -m "message"`.

### Cherry pick

cherry-pick: pick any commit by its reference & appended to the current working HEAD

Cherry-picking a commit pulls a single commit from a different branch and applies it to the current branch.

Sometimes you need to merge only one commit between branches and don’t need to do a full merge. The full merge might be a bad idea because the branch has features that you can’t use yet or other changes that aren’t ready for this branch yet.

```bash
$ git checkout contact
Switched to branch "contact"

$ git commit -m "add link to twitter" -a
Created commit 321d76f: add link to twitter
1 files changed, 4 insertions(+), 0 deletions(-)

$ git checkout master
Switched to branch "master"

$ git cherry-pick 321d76f
Finished one cherry-pick.
Created commit 294655e: add link to twitter
1 files changed, 4 insertions(+), 0 deletions(-)
```

By default, a new commit is created with the changes from that cherry-picked commit.

This is OK in most cases, but what if the code you need is in several commits?  
To cherry-pick multiple commits, give git cherry-pick the `-n` parameter. That tells Git to do the merge but stop before creating a commit.

### Stash

Why you need stash:

- Create a repo, work & commit on `main`
- Switch to another branch and work on it; maybe stage some files
- Some bugs are found on other branch (or even `main`) and you need to fix it _immediately_. Cần phải switch branch nhưng hiện tại chưa commit được thì phải `stash`.

Run the command `git stash` to bring your un-committed changes to the stash (both your modified tracked files and staged changes). After this, you can switch branch.  
When you come back to your branch, run `git stash pop` to bring back changes that you had stashed. You can actually pop changes that are stashed from one branch to a different branch. For example changes stashed in `bugfix` can be pop out in `main`.

You should always first run `git stash list` and only dump the stash you want using something like `git stash apply stash@{0}`

Remember stashing is meant to be used temporarily. And it should be used very carefully when you work with many people.

Mot ứng dụng nữa là đang code mà muốn copy code từ branch khác: stash your changes -> go to other branches for reference, copy some code from other people (Minh Lê) -> go bach to your branch.

The git stash command and git stash push command are functionally equivalent in most common use cases.

- git stash:
  - When executed without any arguments, git stash implicitly performs the action of git stash push. It takes your current uncommitted changes (modified tracked files and staged changes) and stores them in a temporary "stash" list, reverting your working directory to a clean state matching the last commit.
- `git stash push`:
  - This is the explicit command for creating a new stash entry. It offers additional options for more granular control over what is stashed. For instance, you can use:
  * `git stash push -m "Descriptive message"`: To add a custom message to your stash entry, making it easier to identify later.
  * `git stash push --include-untracked`: To also stash untracked files in addition to modified tracked files and staged changes.
  * `git stash push --keep-index`: To stash only the modified tracked files, leaving the staged changes in the index.
  * In essence, git stash push provides the same core functionality as a bare git stash but with the added flexibility of specifying options to control the stashing behavior and provide more descriptive messages. For simple stashing of current changes, either command will achieve the same result.

## git-filter-repo

This is for rewriting an entire repository history.

## Working with Remote

`master` was the default branch name for Git repositories until GitHub changed the default to `main` in October 2020.

We can check out a commit the same way we’d check out branches. Remember, branches are just names for commits.

The refs for local branches are stored in the `./.git/refs/heads/`. Remote branch refs live in the `./.git/refs/remotes/` directory.

**Remote references** are references (pointers) in your remote repositories (not in your local repo), including branches, tags, and so on.  
You can get a full list of remote references explicitly with `git ls-remote <remote>`, or `git remote show <remote>` for remote branches as well as more information  
`git remote -v` show remote repos names and their URLs và loại truy cập (fetch hoặc push)

**Remote tracking branches** are local branch pointers to the state of remote branches. They’re **local** references that you **can’t move**; Git moves them for you whenever you do any network communication. Example `origin/master` show you what the `master` branch on your `origin` remote repo looked like as of the last time you communicated with it. Một ví dụ nữa là `teamone/master` for another remote repo.

- remote tracking branches like `origin/master` only move (update themselves) when you run `git fetch <remote>`
- local branch like `master` move when you commit locally
- Silimar: both are local branch pointer

Với remote tracking branches, `fetch, pull, push` can omit the remote and branch names, as Git automatically knows where to pull from and push to.

Snippet below, có 4 pointer chĩa vào `commit B`. `HEAD` chĩa vào `main` indicate current standing branch. `origin/main` & `origin/HEAD` là tên nhánh giống như `main` chỉ có thêm dấu `/` thôi.

```bash
❯ git log --oneline
d1d9d1d (HEAD -> main, origin/main, origin/HEAD, feature) commit B
6ab407b commit A
```

`git fetch --all` update all your remote-tracking branches like `origin/main` with the online repo.  
`git fetch origin` update & download all branches from the remote as remote tracking branches

`git remote add <name> <URL>` add new remote server (`origin`, `teamone`, `teamtwo`)

`git remote add <your_name_of_choice> <remote_repo_url>` to add a new remote reference.

You can only push to remote servers in which you have write access (your own repo or forked repo). Run `git push <remote> <branch>`

- `git push origin serverfix` => Take my `serverfix` local branch and make it the remote’s `serverfix` branch.
- `git push origin HEAD` => faster than writing out the branch name
- `git push origin serverfix:anhao` => Take my `serverfix` local branch and make it the remote's `anhao` branch. You can use this format to push a local branch into a remote branch that is named differently.

`git push <remote_name> <local_branch_name>` This command will push the local branch `<local_branch_name >` to the remote repo at `< remote_name >`. For example: `git push origin main` push your local `main` branch to your `origin` server (cloning generally sets up both of those names for you automatically).

`git push -u origin main` push into origin from my main. Instead of main, you can push from `production` or maybe `deploy` branch.  
The `-u` option is short for `--set-upstream` and it sets up an upstream that allow you to run future commands of `git push`. Nếu không có upstream thì mỗi lần push phải specify origin & main.

`git push origin main` This command works only if you cloned from a server to which you have write access and if nobody has pushed in the meantime. If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to fetch their work first and incorporate it into yours before you’ll be allowed to push.

You can also do `git push origin serverfix:serverfix`, which does the same thing — it says, “Take my serverfix and make it the remote’s serverfix.” You can use this format to push a local branch into a remote branch that is named differently. If you didn’t want it to be called serverfix on the remote, you could instead run `git push origin serverfix:awesomebranch` to push your local serverfix branch to the awesomebranch branch on the remote project.

Nếu remote's `main` move forward, có commit mới mà local push `origin main` thì sẽ lỗi này:

```bash
$ git push origin main
To github.com:anhaoerv/demo-git.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:anhaoerv/demo-git.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Git nó không có `fetch` lúc mình push. Chỉ đơn giản là lúc push lên nếu git thấy remote có commit mà local chưa có thì nó reject. Lúc này local phải merge những commit đó vào. Nếu chỉ `fetch`, không merge và `push` lên nửa thì cũng bị reject nhưng với một message khác chút xíu. Trong trường hợp bên dưới sau khi `fetch` về rồi `log` ra không còn thấy `origin/main` luôn.

```bash
❯ git fetch
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 939 bytes | 78.00 KiB/s, done.
From github.com:anhaoerv/demo-git
   3512a03..8a02d1e  main       -> origin/main

~/Desktop/demo-git on  main ⇕ ......................................................................................................................3s
❯ git log --oneline -n 6
6ab407b (HEAD -> main) made some changes on mac
3512a03 test ssh on mac
b119f81 Merge branch 'A' into 'main'
253841a B adds new feature
c93f63e A add new feature
fd85cd3 starting point

## vẫn bị reject nhưng message khác
~/Desktop/demo-git on  main ⇕ ........................................................................................................................
❯ git push origin main
To github.com:anhaoerv/demo-git.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:anhaoerv/demo-git.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Lúc này phải `fetch` & `merge origin/main` rồi mới `push` được.

```bash
# khong có commit "made some changes on mac"
❯ git log origin/main --oneline -n 5
8a02d1e (origin/main, origin/HEAD) made some changes to main
3512a03 test ssh on mac
b119f81 Merge branch 'A' into 'main'
253841a B adds new feature
c93f63e A add new feature
```

`git pull` = `git fetch origin` + `git merge origin/main` (resolve nếu có merge conflict). Sau đó `push` lên.

The git remote command is essentially an interface for managing a list of remote entries that are stored in the repository's `./.git/config file`.

`git remote` lists the shortnames of each remote handle you’ve specified.  
To see your remotes & their URLs: `git remote -v` the `-v` option shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote.

`git remove add <NAME> <URL>`. For example `git remote add origin https://github.com/tgoldenphoenix/learn-git.git` the origin is the name.  
Thật ra khi muốn push 1 local repo lên github mình chỉ cần tạo rồi lấy cái URL là xong. Không cần example commands mà nó gợi ý.

`git remote show <remote>` see more information about a particular remote

`git remote rename oldname newname` rename remote

`git remote remove NAME` or `git remote rm` remove a remote

`git branch -d --remote origin/VN_PRO-768` delete remote tracking branh ở local

Two of the easiest ways to access a remote repo are via the HTTP and the SSH protocols. HTTP is an easy way to allow anonymous, read-only access to a repository.

But, it’s generally not possible to push commits to an HTTP address (you wouldn’t want to allow anonymous pushes anyways). For read-write access, you should use SSH instead:

### Tracking branches

It’s important to note that when you do a fetch that brings down new remote-tracking branches, you don’t automatically have local, editable copies of them. In other words, in this case, you don’t have a new `serverfix` branch — you have only an `origin/serverfix` pointer that you can’t modify.
To merge this work into your current working branch, you can run `git merge origin/serverfix`. If you want your own serverfix branch that you can work on, you can base it off your remote-tracking branch:

```git
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

This create a new local **tracking branch** named `serverfix` that you can work on that starts where `origin/serverfix` is.

Checking out a local branch from a remote-tracking branch automatically creates what is called a **tracking branch** (and the branch it tracks is called an **upstream branch** like `origin/hotfix`). Tracking branches are local branches that have a direct relationship to a remote branch. If you’re on a tracking branch and type `git pull`, Git automatically knows which server to `fetch` from and which branch to `merge` in.

- `git push`  # Automatically pushes to the tracked remote branch
- `git pull`  # Automatically pulls from the tracked remote branch

When you clone a repository, it generally automatically creates a `master` branch that tracks `origin/master`. However, you can set up other tracking branches if you wish — ones that track branches on other remotes, or don’t track the master branch. The simple case is the example you just saw, running `git checkout -b <branch> <remote>/<branch>`.

`git branch -vv` list out your local branches with more information including what each branch is tracking and if your local branch is ahead, behind or both.

- **ahead by two** means that we have two commits locally that are not `pushed` to the server
- **ahead by three and behind by one** means that there is one commit on the server we haven’t merged in yet and three commits locally that we haven’t pushed.
- It’s important to note that these numbers are only since the last time you fetched from each server. This command does not reach out to the servers, it’s telling you about what it has cached from these servers locally. If you want totally up to date ahead and behind numbers, you’ll need to fetch from all your remotes right before running this. You could do that like this: `git fetch --all; git branch -vv`.

The HTTP protocol is generally considered the protocol of last resort. It is the least efficient way to pull changes and requires much more net-work overhead, but it is almost always allowed by the strictest firewalls and is relatively quick and easy to set up.

Run `git branch -r` to show remote branches

```bash
prompt> git branch -r
origin/HEAD
origin/master
```

You can check out those branches like a normal branch, but you should not change them. If you want to make a change to them, create a local branch from them first, and then make your change.

Running git fetch updates your remote branches; it doesn’t merge the changes into your local branch.

git pull takes two parameters, the remote repository you want to pull from and the branch you want to pull—without the `origin/` prefix.

Speaking of the origin/ prefix in the remote branch name, that’s to keep the remote branches separate from your local branches. origin is the default remote repository name assigned to a repository that you create a clone from.

Git makes a few assumptions when you call git push without any param-eters. First, it assumes you’re pushing to your origin repository. Second, it assumes you’re pushing the current branch on your repository to its counterpart on the remote repository1 if that branch exists remotely.

Git pushes only what has been checked in, so any changes you have in your working tree or have staged are not pushed

You can also specify the repository you want to push to, just like git pull. The syntax is the same: git push <repository> <refspec>. The <repository> can be any valid repository. Any of the URLs  will work, as will any named repository.

The <refspec> in its simplest form is a tag, a branch, or a special key-word such as HEAD. You can use it to specify which branches to push and where you want them to be pushed. For example, you can use git push origin mybranch:master to push the changes from mybranch to the remote master.

```bash
prompt> git remote add erin git://ourcompany.com/dev-erin.git

prompt> git pull erin HEAD
```

Now, you can use erin instead of the full repository any time you need to push or pull some changes.

### Fetch & Pull

You make your changes in the working area (working directory) -> add them to the staging area -> commit to the local repo -> then push to remote repo. Trước khi push thì mình muốn pull changes made by colleagues on remote về để khi push lên là fresh.

The `git fetch` command downloads commits, files, and refs from a remote repository into your local repo. Fetching is what you do when you want to see what everybody else has been working on. It lets you see how the central history has progressed, but it doesn’t force you to actually merge the changes into your repository. Git isolates fetched content from existing local content; it has absolutely no effect on your local development work. Fetched content has to be **explicitly checked out** using the git checkout command.  
After checking out the fetch, you can `merge` it into your `main`.

`git fetch` simply get the data for you and let you merge it yourself. However, there is a command called git pull which is essentially a git fetch immediately followed by a `git merge` in most cases.

`git fetch <remote>` fetches any new work that has been pushed to that remote server since you cloned (or last fetched from) it, stored it inside your local repo but don't put it in your working area. While `git pull` put the changes into your working area. Nếu không tin tưởng colleagues thì dùng git fetch thôi.  
git pull = git fetch + git merge

While the `git fetch`  command will fetch all the changes on the server that you don’t have yet, it will not modify your working directory at all. It will simply get the data for you and let you merge it yourself. However, there is a command called `git pull` which is essentially a git fetch immediately followed by a git merge in most cases. If you have a tracking branch set up as demonstrated in the last section, either by explicitly setting it or by having it created for you by the clone or checkout commands, git pull will look up what server and branch your current branch is tracking, fetch from that server and then try to merge in that remote branch.

Generally it’s better to simply use the fetch and merge commands explicitly as the magic of git pull can often be confusing.

`git pull origin main` changes will be merged to main. Hoặc dùng `git pull origin` or just `git pull`

git pull fromBranch toBranch (Pulls commits that are different from one branch into another branch so that merging two branches together is easy)

Demo fetch & merge

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

$ git fetch
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 936 bytes | 133.00 KiB/s, done.
From github.com:anhaoerv/demo-git
   4896e14..88c3f0e  main       -> origin/main

$ git log --oneline origin/main
88c3f0e (origin/main, origin/HEAD) add content on github
4896e14 (HEAD -> main) Merge remote-tracking branch 'origin/main'
d26bd16 update readme
c93bf4b Merge pull request #4 from anhaoerv/feature

$ git status
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean

anhao@Anonimous MINGW64 ~/Desktop/demo-git (main)
$ git merge origin/main
Updating 4896e14..88c3f0e
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

### Fork, Pull Request & Open Source contribution

- Head branch: The branch whose changes are combined into the base branch when you merge a pull request. Also known as the "compare branch." Ví dụ `feature`.
- Base branch: `main` or `release`.

Khi làm với repo công ty, người khác:

- fork project về github cá nhân. Nếu có quyền push branch thì clone về luôn khỏi fork.
- clone project vừa fork về máy tính
- add thêm remote url chính (của repo công ty) nếu fork.
- Đứng từ `main` branch, checkout sang branch làm việc mới. Vừa làm vừa rebase vào `main`. Không code trên `main`.
- Làm xong push lên git repo cá nhân, sau đó mở remote công ty tạo **pull request**. Không push `main` mà push cái branch mình làm ví dụ `hotfix, tinh_nang_2` rồi tạo pull request. `main` chỉ có quyền `fetch` về thôi.

Không dùng `git push origin main` mà dùng `git push origin [your branch name]`. Sau khi push rồi thì tạo pull request. Có thể tạo pull request bằng GUI trên github.

- Review pull request
  - Nếu pull request sau khi review thấy cần phải sửa lại code
  - Sửa lại code trên local. `git add`
  - `git commit --amend` trên local
  - `git push origin add_header -f` force push lên cái pull request

Pull request is code reviews.

Open issues để talk to people trước khi viết bất cứ dòng code nào.

To contribute to an open source remote repo: create a fork (or just download the zip file) into your account. Clone the fork from your account to your local machine. Create another branch to work onto it, fix a bug, add a feature. Then you `add`, `commit` on your branch. Then you push `git push origin navbar` (your branch not `main`).

Pull request: Hey I've made some changes and I want to send them changes to the owner of the repo from which I forked. Title & description of the PR must be written carefully. It helps the maintainer to understand what you want to push. Sau khi mình gởi PR rồi thì repo owner bên kia họ sẽ review, check your description, commit diffs, how many files were changed/added. Somebody from the maintainer team must manually go through this. It cannot be automated. Because this is a PR directory to `main`. Maintainer có thể merge PR nếu thấy ổn.

Start from `main` > `checkout -b feature` > add new feature > commit on `feature` > `git push origin feature`

```bash
❯ git log -n 6 --oneline
3d2dff2 (HEAD -> feature) made a new feature
d1d9d1d (origin/main, origin/HEAD, main) Merge remote-tracking branch 'origin/main'
6ab407b made some changes on mac

❯ git push origin feature
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 322 bytes | 322.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote:
remote: Create a pull request for 'feature' on GitHub by visiting:
remote:      https://github.com/anhaoerv/demo-git/pull/new/feature
remote:
To github.com:anhaoerv/demo-git.git
 * [new branch]      feature -> feature
```

Lên Github, chọn `Compare & pull request` hoặc tạo pull request manually. Github sẽ cho mình coi `diff` và merge conflict nếu có.

If the pull request has merge conflicts, you can check out the pull request locally and merge it using the command line.

Tạo branch > commit > push > đã tạo pull request > cần phải sửa pull request, có conflict. Sau đó phải `push -f`

### Creating a pull request from a fork

If you want to create a new branch for your pull request and do not have write permissions to the repository, you can fork the repository first.

## Tagging

A **tag** is like a branch[^1] that doesn’t change. Unlike branches, tags, after being created, have no further history of commits.

`git tag` lists all the tags in alphabetical order; the order in which they are displayed has no real importance.
`git tag -l "v1.8.5*"` If you’re interested only in looking at the 1.8.5 series. Must include `-l` or `--list`

`git tag 1.0 RB_1.0` adds tag name `1.0` to branch `RB_1.0`. "RB" stands for "release branch".

You can create either a lightweight tag or an annotated tag. Annotated tags are generally preferred for releases as they store more information like the tagger's name, email, date, and a message

`git tag -a v1.4 -m "my version 1.4"` create an annotated tag
`git tag -a v1.2 9fceb02` tag commits after you’ve moved past them. You specify the commit checksum (or part of it) at the end of the command
`git tag v1.4-lw` create a lightweight tag, only provide a tag name

`git show <tag name>` show the tag data along with the commit that was tagged

`git push origin <tagname>` push tags to a shared server after you have created them
`git push origin --tags` transfer all of your tags to the remote server that are not already there
`git push <remote> --follow-tags` only annotated tags will be pushed to the remote.

`git tag -d <tagname>` delete tag
`git push origin --delete <tagname>` delete tag from remote server

`git checkout <tag name>` view the versions of files a tag is pointing to

Tags are used to track milestones of the project. They mark a certain point in the history of the repository so you can easily reference them later.

A tag is simply a name that you can use to mark some specific point in the repository’s history. Tags help you keep track of the history of your repository by assigning an easy-to-remember name to a certain revision.

Thường là tạo tag sau đó delete branch. Sau này nếu cần fix có thể tạo branch từ cái tag.

Tags act like bookmarks in your repository. You can use them to jump back to the point in the repository that you tagged. You can tag any commit in Git for any purpose you can think of.

The most common use of tags is to mark when the code in your project is released. That allows you to go back to the code you released if you need to fix or change something later.

Tags in Git are read-only, unlike the tags you might be familiar with if you’re coming from Subversion. This means you can’t make a change to the contents of a tag as if it were a normal branch. This is a much better way to handle tags. You can be sure that the tag is exactly what was tagged and hasn’t changed since it was created.

Có thể checkout ra tags nhưng không thay đổi được. Muốn thay đổi thì từ tag tạo branch.

## git submodule

Sometimes you need to track multiple repositories as if they’re all in the same repository. This might be because of a dependency on some third-party library or possibly because your in-house project has been divided into multiple projects to help make them more manageable.
Git allows you to track external repositories through what it calls sub-modules. These allow you to store a repository within another repository while keeping the two histories completely independent

Once a month, or about every 100 or so commits, it’s a good idea to run `git gc` to tidy things up by optimizing the way Git stores its history internally. It doesn’t change the history, only the way it is stored.

The --onto parameter provides another interesting way to rewrite your history. For example, you have three branches: master, the contacts branch that was created from master, and the search branch that was created from contacts.
The history of those branches might go something like this. You started with contact code, but partway into it you decide to add new search code too.
After you finish with the new search code, you realize that it would work directly without requiring any of the changes that are in your contact branch.
This is what the --onto parameter does. It takes one parameter, the branch you want to rebase onto. Your command to rebase search onto the master branch looks like this:
prompt> git rebase --onto master contact search

This breaks the search branch off the contact branch and moves it to the master branch. This is useful if you need to merge the search branch back into master but don’t need everything in the contact branch. Of course, search has to be completely independent to keep from having any merge conflicts when the rebase starts replaying the changes.

## Git Aliases

Here are some example:

- `git config --global alias.co checkout`
- `git config --global alias.br branch`
- `git config --global alias.ci commit`
- `git config --global alias.st status`

## Github

Issues & Github Projects: Bare-bones project management

Github Actions: Automated build/release pipelines.

[Gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists/creating-gists) provide a simple way to share **code snippets** with others. Every gist is a Git repository, which means that it can be forked and cloned. If you are signed in to GitHub when you create a gist, the gist will be associated with your account and you will see it in your list of gists when you navigate to your gist home page.

A [codespace](https://docs.github.com/en/codespaces/overview) is a development environment that's hosted in the cloud. You can customize your project for GitHub Codespaces by committing configuration files to your repository (often known as Configuration-as-Code), which creates a repeatable codespace configuration for all users of your project. See "Introduction to dev containers."

[stack overflow](https://stackoverflow.com/questions/23611669/how-to-find-the-created-date-of-a-repository-project-on-github) How to find the created date of a repository project on GitHub? using github api

## Other techniques

Nhánh `development` sẽ được merge vào nhánh `release`

Người dùng sẽ sử dụng nhánh `main/master`. Nhánh `release` chứa những commits được scheduled để merge vào `main`.

Nhánh `hotfix` chỉ nhảy ra từ `main` hoặc `release`

## Git principles

commit often & commit small. Viết ra cái gì xong & code vẫn build run OK thì cứ commit.

Every new idea for the same problem require a new branch.

## Git Internals

The SHA-1 checksum is a 40 hexadecimal digits number.

## Git Hooks

Hooks are scripts that are executed automatically in certain conditions.

## Further readings

[git from the bottom up](https://jwiegley.github.io/git-from-the-bottom-up/) learn the internal of git (blobs, trees, commits, and refs). How git organize its database.
