# Git & github notes

Bitbucket password: ATBBtwDKEzmAzNEatPMnZCEada380D70D48C
username: tGoldenPhoenix

Check if Git is installed locally: `git --version`

**Getting help**: `git help <verb>`, for example `git help config`  
`git <verb> -h` show a short list of available options

## git config

Show git config: `git config --list`  
You can view all of your settings and where they are coming from using:: `git config --list --show-origin`  
The global gitconfig file on Macbook is at: `/opt/homebrew/etc/gitconfig`

Mới tải Git về thì run these commands:

- `git config --global user.name "John Doe"`
- `git config --global user.email johndoe@example.com`

To set `main` as the default branch name do: `git config --global init.defaultBranch main`  
GitHub changed the default branch name from `master` to `main` in mid-2020

Using Visual Studio Code as your default git editor: `git config --global core.editor "code --wait"`

Phải có cả authentication key & signing key mới push lên github được (I think so).

## Initializing a new repository

`git init` (automatically create the `master or main` branch). Executing this command will create a new `.git` subdirectory in your current working directory.

`git clone <repo url>`

- automatically creates a remote called `origin` for fetch, push, pull
- Create local **Remote Tracking Branches** to track the corresponding branches on the remote repository (e.g., `origin/main` for the `main` branch on the `origin` remote).
- Create a local `main` starting at the same place as `origin/main` and configure it to track the corresponding remote branch (e.g., `origin/main`). Khi `pull push` thuận tiện hơn

`origin` is the default name for a remote when you run `git clone`. If you run `git clone -o booyah` instead, then you will have `booyah` as your remote name instead of `origin`.

Git SSH URLs follow a template of: `git@HOSTNAME:USERNAME/REPONAME.git`. Host name là Github or bitbucket

## git diff, view changes

There are 2 _states_ of files:

- **tracked**: files that were in the last snapshot, as well as any newly staged files; they can be unmodified, modified, or staged.
- **untracked**: everything else

`git status` show file states, merge conflicts if exists  
`git status -uall <file/folder>`: Nếu muốn git status show files trong folders (mặc định chỉ show folder) thì [here](https://stackoverflow.com/questions/28222633/git-status-not-showing-contents-of-newly-added-folder)

`git diff` compares what is in your **working directory** with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged with `git add`.  
`git diff` does not compare file A and file B (there are different tool for that). It compare the same file A in different stages: same file on different commits, staged file and the latest commit, same file but on different branch.

[how to read git diff output](https://stackoverflow.com/questions/2529441/how-to-read-the-output-from-git-diff)

`---` & `+++` do NOT means added & remove the code. They only indicate changes in the file:

- The `---` represent file `a->` (not necessarily the previous version. It's just the file a).
- The `+++` represent file `b->`. Note that `a->` and `b->` are the same file over time

Compares your staged changes to your last commit `git diff --staged` or `git diff --cached`.

Git diff two commit `git diff a498f47 7274899`. Or you can use the older `..` operator instead of space character such as `git diff a498f47..7274899`.  
Nên để 2 cái commit id theo đúng thứ tự thời gian `older..newer`. Nếu để commit ngược lại thì sẽ khó hiểu lắm.

`git diff branchOne..branchTwo`

## git stash

Why you need stash:

- Create a repo, work & commit on `main`
- Switch to another branch and work on it; maybe stage some files
- Some bugs are found on other branch (or even `main`) and you need to fix it _immediately_. But, the thing is, git won't let you switch branch if there are some tracked or untracked changes. You must either commit of stash those changes before switching branch.

Run the command `git stash` to bring your un-committed changes to the stash. After this, you can switch branch.  
When you come back to your branch, run `git stash pop` to bring back changes that you had stashed. You can actually pop changes that are stashed from one branch to a different branch. For example changes stashed in `bugfix` can be pop out in `main`.

You should always first run `git stash list` and only dump the stash you want using something like `git stash apply stash@{0}`

Remember stashing is meant to be used temporarily. And it should be used very carefully when you work with many people.

Mot ứng dụng nữa là đang code mà muốn copy code từ branch khác: stash your changes -> go to other branches for reference, copy some code from other people (Minh Lê) -> go bach to your branch.

## Add & Commit

`git add *` does not add deleted files. Chỉ nên dùng `git add .` or `git add -A`.

`git add` is also used to mark merge conflicts as resolved.

- `git commit` launches your shell editor. It is also used to **conclude merges**
- `git commit -v` puts the diff of your change in the shell editor.
- `git commit -m <message>` không mở shell editor.

`git commit -a -m 'Add new benchmarks'`. The `-a` option automatically stage every file that is already tracked before doing the commit, letting you skip the `git add` part. You can use `-am` or `-a -m`

### Moving, renaming & removing files

`git rm file.txt` remove the file from your tracked files and also removes the file from your working directory.

`git rm --cached README` _keep the file in your working tree_ but remove it from your staging area.

Lỡ add, push lên github một folder rồi nhưng sau đó lại muốn bỏ thì sau khi cho vào `.gitignore` rồi phải run `git rm` nhé

`git mv file_from file_to` **rename** a file in Git.

### Viewing the commit history

`git log` the most recent commits show up first.  
`git log --oneline`

`git log -p -2`: The `-p, --patch` option shows the difference (the patch output) introduced in each commit. You can also limit the number of log entries displayed, such as using `-2` to show only the last two entries.
The `--stat` shows some abbreviated stats for each commit.

`git log --pretty=oneline` prints each commit on a single line, which is useful if you’re looking at a lot of commits.
`--pretty=format` allows you to specify your own log output format

`--graph` adds a nice little ASCII graph showing your branch and merge history

`--since`, `--until`: time-limiting options (useful)

`--author`, `--grep` filter options (useful)

`-S` takes a string and shows only those commits that changed the number of occurrences of that string

Each commit bao gồm: unique hash, pointer to its parent (except the first commit) & commit info (time, user email/name)

### Commit message

How to write atomic commit messages:
one commit for one change. 10 bugs -> 10 commits  
present tense, imperative; give order to codebase; no case

### Git ignore

Những thông tin phải `.gitignore` không được push lên remote repositories:

- API keys
- password
- AWS key, open AI keys can get you bankrupt

You can google `gitignore generator` for templates for different kind of project.

## Undoing Things

Remember, anything that is committed in Git can almost always be recovered. Even commits that were on branches that were deleted or commits that were overwritten with an --amend commit can be recovered (see Data Recovery for data recovery). However, anything you lose that was never committed is likely never to be seen again.

`git commit --amend` re-do the latest commit. **NOTE:** Only amend commits that are still local and have not been pushed somewhere.

- You can change commit message, add & change staged files
- You end up with a single commit — the second commit replaces the results of the first.

`git restore <file>` to discard changes in modified files and revert them back to the latest commit snapshot version (un-modified a file). :bangbang: You will lose all your local changes to that file (`git checkout` is an old version of this).  
You can check with the checkout command that how files look like but you can only restore the file to the last commit only. You cannot go further.

`git restore --staged <file>` to unstage file (`git reset` is an old version of this). Or `git rm --cache <file>` to unstage

## Github

Issues & Github Projects: Bare-bones project management

Github Actions: Automated build/release pipelines.

## Fork, Pull Request and Open Source contribution

Khi làm với repo công ty, người khác:

- fork project về github cá nhân
- clone project vừa fork về máy tính
- add thêm remote url chính (của repo công ty)
- Đứng từ main branch, checkout sang branch làm việc mới. Làm xong rồi merge vào master. Rồi lại checkout ra làm funtion khác. Không code trực tiếp vào master.
- Làm xong push lên git repo cá nhân, sau đó mở remote công ty tạo **pull request**. Không push `main` mà push cái branch mình làm ví dụ `hotfix, tinh_nang_2`

Không dùng `git push origin main` mà dùng `git push origin [your branch name]`. Sau khi push rồi thì tạo pull request. Có thể tạo pull request bằng GUI trên github.

Review pull request

- Nếu pull request sau khi review thấy cần phải sửa lại code
- Sửa lại code trên local. `git add`
- `git commit --amend` trên local
- `git push origin add_header -f` force push lên cái pull request

Pull request is code reviews.

Open issues để talk to people trước khi viết bất cứ dòng code nào.

To contribute to an open source remote repo: create a fork (or just download the zip file) into your account. Clone the fork from your account to your local machine. Create another branch to work onto it, fix a bug, add a feature. Then you `add`, `commit` on your branch. Then you push `git push origin navbar` (your branch not `main`).

Pull request: Hey I've made some changes and I want to send them changes to the owner of the repo from which I forked. Title & description of the PR must be written carefully. It helps the maintainer to understand what you want to push. Sau khi mình gởi PR rồi thì repo owner bên kia họ sẽ review, check your description, commit diffs, how many files were changed/added. Somebody from the maintainer team must manually go through this. It cannot be automated. Because this is a PR directory to `main`. Maintainer có thể merge PR nếu thấy ổn.

Reference:

[Trung quân dev hướng dẫn git](https://www.youtube.com/watch?v=swlrBlriFPE&list=PLP6tw4Zpj-RLzH_NrF8j6SvZLuPk6t948&index=4)

Basic Git workflow with a remote hosting service [here](https://www.atlassian.com/git)

[Solo Development Use Cases](https://www.youtube.com/watch?v=NkIAMP8uTik&t=262s)

[Github Workflow for a Single Developer Project](https://www.youtube.com/watch?v=5smG5Dr-93E)

[cách contribute to open source](https://www.youtube.com/watch?v=zTjRZNkhiEU) by Hitesh Choudhary

[folder practice hosted by Hitesh Choudhary](https://github.com/hiteshchoudhary/open-source-contribution)

## Tagging

A **tag** is like a branch[^1] that doesn’t change. Unlike branches, tags, after being created, have no further history of commits.

`git tag` lists all the tags in alphabetical order; the order in which they are displayed has no real importance.
`git tag -l "v1.8.5*"` If you’re interested only in looking at the 1.8.5 series. Must include `-l` or `--list`

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

## Git Aliases

Here are some example:

- `git config --global alias.co checkout`
- `git config --global alias.br branch`
- `git config --global alias.ci commit`
- `git config --global alias.st status`

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

`git branch <branch name>` creates a **new branch** which points to the same commit you’re currently on (`HEAD` point to it). Note that this command does not switch to the newly created branch.  
Now there are two branches point to the same commit object.

- `git branch` list **local** branches. The current local branch will be marked with an asterisk (the branch that `HEAD` points to).
- To see all **remote branch** names, run `git branch -r`
- To see all local and remote branches, run `git branch -a`
- `git branch -v` see the last commit on each branch.
- `git branch --merged` see which branches are already merged into the branch you’re on. Branches on this list without the `*` in front of them are generally fine to delete.
- `git branch --no-merged` see all the branches that contain work you haven’t yet merged in

- `git log --oneline --decorate` The `--decorate` option shows you where the branch pointers are pointing
- `git log --oneline --decorate --graph --all` print out the history of your commits, showing where your branch pointers are and how your history has diverged.

By default, `git log` will only show commit history below the branch you’ve checked out.  
To show commit history for the desired branch you have to explicitly specify it: `git log testing`.  
To show all of the branches, add `--all` to your `git log` command.

Switch to an existing branch `git checkout <branch name>`. This effectively moves `HEAD` to point to `<branch name>` branch and reverts the files in your working directory back to the snapshot that branch points to.  
`git checkout -b <newbranchname>` create a new branch and switch to that new branch in just one command. Giống `-c` của git switch.

`git checkout <HASH-f9f4f1c>` will put you in detach HEAD. You get to the detached HEAD state by checking out a commit directly instead of a branch name.

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

### Merging branches

Khi muốn bring changes from one branch (`main`) to your personal branch cũng gọi là merge.

Các trường hợp merge như sau:

1. fast-forward: create a branch and work on it để nếu có gì hỏng thì return back to `main`. Git just move the branch pointer forward no new commit (snapshot) created. Khi merge will have no conflict.
2. Three-way merge: using the two snapshots pointed to by the branch tips and the common ancestor of the two. Sẽ có conflict nếu 2 branches tips changed the same file. The changes do not necessarily need to the on the same line. It only need to be on the same file, to create merge conflict. Git creates a new commit (snapshot) called **merge commit**. This merge commit points to two parent while normal commit only has one parent.

Nếu có merge conflict thì git không tạo merge commit mà pause để user resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run `git status`. Git auto add **conflict-resolution markers** to the files that have conflicts, so you can open them manually and resolve those conflicts.

Phân biệt git diff marker & git conflict markers.

[Git conflict markers](https://stackoverflow.com/questions/7901864/git-conflict-markers).  
Keep whatever you want, remove markers & save. You can manually remove the markers `<<<<<` & `=====` or some editor provide buttons that you can just click.  
Sau khi resolve merge conflict manually in your editor, run `git add` on each file to mark it as resolved. Staging the file marks it as resolved in Git. Run `git status` again to verify that all conflicts have been resolved. Sau đó `git commit -m "resolve merge conflict"`.

Merge 2 branches làm 1 và fastforward đều dùng chung 1 command: Check out the branch you wish to **merge into** (usually `main/master`) and then run the `git merge [branch-name]` command. Think "I'm bringing somthing into me so I want to stand on the final destination".  
Khi fast-forward merge cái branch mình cần merge sẽ đi trước `main` nhưng mình vẫn sẽ checkout về `main`.  
Vì merge branch sẽ tạo ra 1 commit mới nên nó cũng cần có message giống như khi mình commit. Fast-forward thì nó không hỏi mà lấy luôn commit của lastest snapshot.

Ví dụ merge hotfix into main: `git merge hotfix-branch` when standing on `main` branch even though `main` is behind `hotfix-branch`.

Khi dùng `git log --oneline` nếu có visual tool like [VSCode git graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) sẽ dễ hình dung hơn. Merge 2-> 1 có conflict hay không thì khi nhìn graph timeline cũng giống y chang nhau.

Delete branch: `git branch -d <branch name>`. You delete branch after merging successfully.

`git mergetool` fires up an appropriate visual merge tool and walks you through the conflicts

### Changing a branch name

`git branch --move bad-branch-name corrected-branch-name` This replaces your bad-branch-name with corrected-branch-name, but this change is only local for now. To let others see the corrected branch on the remote, push it `git push --set-upstream origin corrected-branch-name`

`git branch --all` List both remote-tracking branches and local branches

`git push origin --delete bad-branch-name` delete bad-branch-name remote branch

### Changing the master branch name

`git branch --move master main` Rename your local master branch into main. To let others see the new main branch, you need to push it to the remote. This makes the renamed branch available on the remote: `git push --set-upstream origin main`
`git push origin --delete master` delete the master branch on remote

### Remote Branches

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

`git fetch --all` update all your remote-tracking branches like `origin/main` with the online repo.  
`git fetch origin` update & download all branches from the remote as remote tracking branches

`git remote add <name> <URL>` add new remote server (`origin`, `teamone`, `teamtwo`)

`git remote add <your_name_of_choice> <remote_repo_url>` to add a new remote reference.

You can only push to remote servers in which you have write access (your own repo or forked repo). Run `git push <remote> <branch>`

- `git push origin serverfix` => Take my `serverfix` local branch and make it the remote’s `serverfix` branch.
- `git push origin serverfix:anhao` => Take my `serverfix` local branch and make it the remote's `anhao` branch. You can use this format to push a local branch into a remote branch that is named differently.

`git push <remote_name> <local_branch_name>` This command will push the local branch `<local_branch_name >` to the remote repo at `< remote_name >`. For example: `git push origin master` push your local `master` branch to your `origin` server (cloning generally sets up both of those names for you automatically).

`git push -u origin main` push into origin from my main. Instead of main, you can push from `production` or maybe `deploy` branch.  
The `-u` option is short for `--set-upstream` and it sets up an upstream that allow you to run future commands of `git push`. Nếu không có upstream thì mỗi lần push phải specify origin & main.

`git push origin main` This command works only if you cloned from a server to which you have write access and if nobody has pushed in the meantime. If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to fetch their work first and incorporate it into yours before you’ll be allowed to push.

You can also do `git push origin serverfix:serverfix`, which does the same thing — it says, “Take my serverfix and make it the remote’s serverfix.” You can use this format to push a local branch into a remote branch that is named differently. If you didn’t want it to be called serverfix on the remote, you could instead run `git push origin serverfix:awesomebranch` to push your local serverfix branch to the awesomebranch branch on the remote project.

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

## Pushing code to github & working with remotes

SSH key is stored in `/Users/anhao/.ssh/`. Trong này chứa cả private & public key pair. Cái file `.pub` là public key còn cái file `id_ed25519` là private key.

With SSH keys, if someone gains access to your computer, the attacker can gain access to every system that uses that key. To add an extra layer of security, you can add a passphrase to your SSH key. To avoid entering the passphrase every time you connect, you can securely save your passphrase in the SSH agent.

When you set up SSH, you will need to generate a new private SSH key and add it to the SSH agent on your local machine. You must also add the public SSH key to your account on GitHub before you use the key to authenticate or sign commits.

When you generate an SSH key, you can add a **passphrase** to further secure the key. Whenever you use the key, you must enter the passphrase. If your key has a passphrase and you don't want to enter the passphrase every time you use the key, you can add your key to the SSH agent. The SSH agent manages your SSH keys and remembers your passphrase.

If you haven't used your SSH key for a year, then GitHub will automatically delete your inactive SSH key as a security precaution.

`git branch -M main` is used to change your branch name from master -> main.

The git remote command is essentially an interface for managing a list of remote entries that are stored in the repository's `./.git/config file`.

`git remote` lists the shortnames of each remote handle you’ve specified.  
To see your remotes & their URLs: `git remote -v` the `-v` option shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote.

`git remove add <NAME> <URL>`. For example `git remote add origin https://github.com/tgoldenphoenix/learn-git.git` the origin is the name.  
Thật ra khi muốn push 1 local repo lên github mình chỉ cần tạo rồi lấy cái URL là xong. Không cần example commands mà nó gợi ý.

`git remote show <remote>` see more information about a particular remote

`git remote rename oldname newname` rename remote

`git remote remove NAME` or `git remote rm` remove a remote

Two of the easiest ways to access a remote repo are via the HTTP and the SSH protocols. HTTP is an easy way to allow anonymous, read-only access to a repository.

But, it’s generally not possible to push commits to an HTTP address (you wouldn’t want to allow anonymous pushes anyways). For read-write access, you should use SSH instead:

### Fetch & Pull

You make your changes in the working area (working directory) -> add them to the staging area -> commit to the local repo -> then push to remote repo. Trước khi push thì mình muốn pull changes made by colleagues on remote về để khi push lên là fresh.

The `git fetch` command downloads commits, files, and refs from a remote repository into your local repo. Fetching is what you do when you want to see what everybody else has been working on. It lets you see how the central history has progressed, but it doesn’t force you to actually merge the changes into your repository. Git isolates fetched content from existing local content; it has absolutely no effect on your local development work. Fetched content has to be explicitly checked out using the git checkout command.

`git fetch` simply get the data for you and let you merge it yourself. However, there is a command called git pull which is essentially a git fetch immediately followed by a `git merge` in most cases.

`git fetch <remote>` fetches any new work that has been pushed to that remote server since you cloned (or last fetched from) it, stored it inside your local repo but don't put it in your working area. While `git pull` put the changes into your working area. Nếu không tin tưởng colleagues thì dùng git fetch thôi.\
git pull = git fetch + git merge

While the `git fetch`  command will fetch all the changes on the server that you don’t have yet, it will not modify your working directory at all. It will simply get the data for you and let you merge it yourself. However, there is a command called `git pull` which is essentially a git fetch immediately followed by a git merge in most cases. If you have a tracking branch set up as demonstrated in the last section, either by explicitly setting it or by having it created for you by the clone or checkout commands, git pull will look up what server and branch your current branch is tracking, fetch from that server and then try to merge in that remote branch.

Generally it’s better to simply use the fetch and merge commands explicitly as the magic of git pull can often be confusing.

`git pull origin main` changes will be merged to main. Hoặc dùng `git pull origin` or just `git pull`

## Rebase

In Git, there are two main ways to integrate changes from one branch into another: the merge and the rebase.  
The rebase command can mess up your project. Rebasing re-write the commit history. You therefore shall use it carefully. You must know what you are doing.

rebase can be used as:

- alternative to merging
- clean up tool (clean up commits) because it re-write the history

With the rebase command, you can take all the changes that were committed on one branch and re-apply them on a different branch.

You would check out the `experiment` branch, and then rebase it onto the `master` by running:

```bash
git checkout experiment
git rebase master
```

Nếu merge thì checkout qua `master`

There is no difference in the end product of the integration, but rebasing makes for a cleaner history. If you examine the log of a rebased branch, it looks like a linear history: it appears that all the work happened in series, even when it originally happened in parallel.

If you are on the `main/master` branch, **NEVER** run `git rebase`. It is meant to be run when you are on the branches (bugfix, footer) not the main.  
NEVER rebase commits that you have shared, pushed to Github.  
Khi làm công ty thì phải hỏi cấp trên.

In Git, there are two main ways to integrate changes from one branch into another: the merge and the rebase.  
Very often you will want to bring changes from `main` to my personal branch. Merge main into our personal branch allows you to see the latest update. Đây là một thao tác rất thường hay làm. Nhưng nếu dùng `merge` thì nó có một tác dụng phụ là commit log sẽ có nhiều merge commit. Merge commits have no real meaning, they are just merging. Chính vì vậy nên người ta mới dùng `rebase`.

Rebasing your branch with the master `git rebase main`. You don't want to mess up the main branch.

Khi rebase nếu có conflict thì cũng resolve giống như merge bình thường. Nhớ phải làm đúng như nó kêu. Phải resolve conflict manually, rồi `add` rồi run `git rebase --continue`

## Git Internals

The SHA-1 checksum is a 40 hexadecimal digits number.

## Git Hooks

Hooks are scripts that are executed automatically in certain conditions.

## Other Github features

[Gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists/creating-gists) provide a simple way to share **code snippets** with others. Every gist is a Git repository, which means that it can be forked and cloned. If you are signed in to GitHub when you create a gist, the gist will be associated with your account and you will see it in your list of gists when you navigate to your gist home page.

A [codespace](https://docs.github.com/en/codespaces/overview) is a development environment that's hosted in the cloud. You can customize your project for GitHub Codespaces by committing configuration files to your repository (often known as Configuration-as-Code), which creates a repeatable codespace configuration for all users of your project. See "Introduction to dev containers."

[stack overflow](https://stackoverflow.com/questions/23611669/how-to-find-the-created-date-of-a-repository-project-on-github) How to find the created date of a repository project on GitHub? using github api

## Terminology

## References

[How to Use Git and GitHub in a Team like a Pro – Featuring Harry and Hermione 🧙](https://www.freecodecamp.org/news/how-to-use-git-and-github-in-a-team-like-a-pro/)

[how to git push](https://devconnected.com/how-to-push-git-branch-to-remote/) mình dùng trong edunext

atlassian.com/git

[How GIT works under the HOOD?](https://www.youtube.com/watch?v=RxHJdapz2p0&t=410s)

[learn git by Hitesh Choudhary](https://www.youtube.com/watch?v=zTjRZNkhiEU) beginner friendly, very good.
