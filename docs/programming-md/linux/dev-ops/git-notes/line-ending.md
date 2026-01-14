# Line Endings (CR & LF terminators)

## The Problem

Computers need to know when to start a new line, but the characters used to represent this can vary between platforms. this causes compatibility issues and unforeseen side effects, such as giant `git diff`s with no apparent change.

- There are 2 control characters that represent line endings:
- carriage return
  - aka `CR` `\r` `^M` `0x0d`
  - represents a cursor movement back to column 0
- line feed
  - aka `LF` `\n` `$` `0x0a`
  - represents a cursor movement directly downward

- Windows uses CR + LF to represent line-endings
- Any unix-like operating operating system (linux, BSD, macOS) represents line-endings with a single `LF`.
- This makes `git diff` messy. Git’s primary solution to all this is to specify that `LF` is the best way to store line endings for text files in a Git repository’s object database. It doesn’t force this on you but most developers using Git and GitHub have adopted this as a convention and even our own help recommends setting up your config to do this.

## How to check line-endings

the `file` command will explicitly state `CRLF` endings; `LF` is implicit.

```shell
$ file lf.txt
lf.txt: ASCII text

$ file crlf.txt
crlf.txt: ASCII text, with CRLF line terminators
```

use `cat` with the `-v` or `-e` option

- `^M`=`CR`
- `$`=`LF`

```shell
$ cat -v crlf.txt # hides LF
never gonna give you up^M
never gonna let you down^M

$ cat -e crlf.txt # shows LF
never gonna give you up^M$
never gonna let you down^M$
```

In VS Code, the current file's line ending (CRLF or LF) is displayed in the bottom-right corner of the status bar.

## Fix line-endings for one-off files

- use a tool like
  - `dos2unix` / `unix2dos`
  - `vim` `sed` `tr` etc...

most reliable is probably `dos2unix` ([may require install](https://formulae.brew.sh/formula/dos2unix))

```shell
dos2unix crlf.txt # convert to LF
unix2dos lf.txt   # convert to CRLF
```

some other ways

```shell
sed 's/\r$//' crlf.txt > lf.txt
tr -d '\r' < crlf.txt > lf.txt
```

## To Configure git (+repos)

configure git locally to use to `LF` endings

```shell
git config --global core.autocrlf true
```

configure line endings per-project **(RECOMMENDED)**

create a `.gitattributes` file in project root dir

```shell
* text=auto
```

- i recommend starting with [a gitattributes template](https://github.com/alexkaratarakis/gitattributes)
- **to understand** `warning: LF will be replaced by CRLF`, [refer to this stackoverflow answer](https://stackoverflow.com/questions/1967370/git-replacing-lf-with-crlf)

to change endings in an EXISTING project:

```shell
# save files before changing
$ git add . -u
$ git commit -m "Saving files before refreshing line endings"

# renormalize endings (make sure run from project root)
$ git add --renormalize .

# check and commit
$ git status
$ git commit -m "normalize line endings"
```

## Fix line-ending in git

### `core.eol`

You should not change `core.eol`, leave default. This setting doesn’t do much on its own, but as soon as we start telling Git to change our line endings for us we need to know the value of `core.eol`. This setting is used by all the other things we are going to talk about below, so it’s good to know that it exists and good to know that you probably don’t want to change it.

- `core.eol = native` The default. When Git needs to change line endings to write a file in your working directory it will change them to whatever is the default line ending on your platform. For Windows this will be CRLF, for Unix/Linux/OS X this will be LF.
- `core.eol = crlf` When Git needs to change line endings to write a file in your working directory it will always use CRLF to denote end of line.
- `core.eol = lf` When Git needs to change line endings to write a file in your working directory it will always use LF to denote end of line.

You can run `git config --global core.eol` to see what this value is set to on your system. If nothing comes back that means you are on the using the default which is native.

### In and out of the object database

Git has its own database in that `.git` folder

when you do something like `git commit` you are writing objects into the database. This involves taking the files that you are committing, calculating their shas and writing them into the object database as blobs. This is what I mean when I say **writing to the object database** and this is when Git has a chance to run filters and do things like converting line endings.

The other place that Git has a chance to run filters is when it reads out of the object database and writes files into your working directory. This is what I mean when I say **writing out into the working directory**. Many commands in Git do this, but `git checkout` is the most obvious and easy to understand. This also happens when you do a `git clone` or run a command like `git reset` that changes your working directory.

### The Old System: `core.autocrlf`

The `old system` is the original set of features in Git designed to solve this particular problem of line endings. These are global settings set on local machines of each developer in the team.

Git has a configuration setting called `core.autocrlf` which is specifically designed to make sure that when a text file is written to the repository’s object database that all line endings in that text file are normalized to LF. Here are the different options for `core.autocrlf` and what they mean:

- `core.autocrlf = false` This is the default, but most people are **encouraged to change this immediately**. The result of using `false` is that Git doesn’t ever mess with line endings on your file. You can check in files with `LF` or `CRLF` or `CR` or some random mix of those three and Git does not care. This can make **diffs harder to read and merges more difficult**. Most people working in a Unix/Linux world use this value because they don’t have `CRLF` problems and they don’t need Git to be doing extra work whenever files are written to the object database or written out into the working directory.
- `core.autocrlf = true` This means that Git will process all text files and make sure that `CRLF` is replaced with `LF` when writing that file to the object database and turn all `LF` back into `CRLF` when writing out into the working directory. This is the **recommended setting on Windows** because it ensures that your repository can be used on other platforms while retaining `CRLF` in your working directory.
- `core.autocrlf = input` This means that Git will process all text files and make sure that CRLF is replaced with LF when writing that file to the object database. It will **NOT**, however, do the reverse. When you read files back out of the object database and write them into the working directory they will still have LFs to denote the end of line. This setting is generally used on Unix/Linux/OS X to prevent CRLFs from getting written into the repository. The idea being that if you pasted code from a web browser and accidentally got CRLFs into one of your files, Git would make sure they were replaced with LFs when you wrote to the object database.

You can run `git config --global core.autocrlf` to see what this value is set to on your system. If nothing comes back that means you are on the using the default which is `false`.

How does Git know that a file is _text_? Good question. Git has an internal method for heuristically checking if a file is binary or not. A file is deemed _text_ if it is not binary. Git can sometimes be wrong and this is the basis for our next setting.

The next setting that was introduced is `core.safecrlf` which is designed to protect against these cases where Git might change line endings on a file that really should just be left alone.

- `core.safecrlf = true` - When getting ready to run this operation of replacing `CRLF` with `LF` before writing to the object database, Git will make sure that it can actually successfully back out of the operation. It will verify that the reverse can happen (`LF` to `CRLF`) and if not the operation will be aborted.
- `core.safecrlf = warn` - Same as above, but instead of aborting the operation, Git will just warn you that something bad might happen.

---

One final layer on all this is that you can create a file called `.gitattributes` in the root of your repository and add rules for specific files. These rules allow you to control things like `autocrlf` on a per file basis. So you could, for instance, put this in that file to tell Git to always replace `CRLF` with `LF` in txt files:

```txt
*.txt crlf
```

Or you could do this to tell Git to never replace `CRLF` with `LF` for txt files like this:

```txt
*.txt -crlf
```

Or you could do this to tell Git to only replace `CRLF` with `LF` when writing, but to read back `LF` when writing the working directory for txt files like this:

```txt
*.txt crlf=input
```

### The new System

Enter the new system which is available in Git 1.7.2 and above.

The new system moves to defining all of this in the `.gitattributes` file that you keep with your repository. This means that line endings can be encapsulated entirely within a repository and don’t depend on everyone having the proper global settings.

In the new system you are in charge of telling git which files you would like `CRLF` to `LF` replacement to be done on. This is done with a `text` attribute in your repository’s `.gitattributes` file. In this case the man page is actually quite helpful. Here are some examples of using the `text` attribute:

- `*.txt text` Set all files matching the filter `*.txt` to be text. This means that Git will run `CRLF` to `LF` replacement on these files every time they are written to the object database and the reverse replacement will be run when writing out to the working directory.
- `*.txt -text` Unset all files matching the filter. These files will never run through the `CRLF` to `LF` replacement.
- `*.txt text=auto` Set all files matching the filter to be converted (`CRLF` to `LF`) **if** those files are determined by Git to be text and not binary. This relies on Git’s built in binary detection heuristics.

If a file is unspecified then Git falls back to the `core.autocrlf` setting and you are back in the old system. This is how backwards compatibility is maintained, but I would recommend (especially for Windows developers) that you explicitly create a `.gitattributes` file.

Here is an example you might use for a C# project:

```txt
# These files are text and should be normalized (convert crlf =&gt; lf)
*.cs      text diff=csharp
*.xaml    text
*.csproj  text
*.sln     text
*.tt      text
*.ps1     text
*.cmd     text
*.msbuild text
*.md      text

# Images should be treated as binary
# (binary is a macro for -text -diff)
*.png     binary
*.jepg    binary

*.sdf     binary
```

One final note that the man page for gitattributes mentions is that you can tell git to detect _all_ text files and automatically normalize them (convert `CRLF` to `LF`):

```txt
*       text=auto
```

This is certainly better than requiring everyone to be on the same global setting for `core.autocrlf`, but it means that you really trust Git to do binary detection properly. In my opinion it is better to explicitly specify your text files that you want normalized. Don’t forget if you are going to use this setting that it should be the first line in your `.gitattributes` file so that subsequent lines can override that setting.

### warning: LF will be replaced by CRLF

On a Windows machine, I added some files using git add. I got warnings saying:

> LF will be replaced by CRLF

When does this warning show up (under Windows)?

- `autocrlf = true` if you have unix-style lf in one of your files (= RARELY),
- `autocrlf = input` if you have win-style crlf in one of your files (= almost ALWAYS),
- `autocrlf = false` – NEVER!

What does this warning mean?

- The warning "LF will be replaced by CRLF" says that you (having `autocrlf=true`) will lose your unix-style LF after commit-checkout cycle (it will be replaced by windows-style CRLF). Git doesn't expect you to use unix-style LF under Windows.
- The warning "CRLF will be replaced by LF" says that you (having autocrlf=input) will lose your windows-style CRLF after a commit-checkout cycle (it will be replaced by unix-style LF). Don't use `input` under Windows.

How to fix

- The default value for `core.autocrlf` is selected during Git installation and stored in system-wide gitconfig (`\Program Files\Git\etc\gitconfig` on Windows, `/etc/gitconfig` on Linux). Also there are (cascading in the following order):
  - "global" (per-user) gitconfig located at `~/.gitconfig`, yet another
  - "global" (per-user) gitconfig at `$XDG_CONFIG_HOME/git/config` or $HOME/.config/git/config and
  - "local" (per-repo) gitconfig at `.git/config` in the working directory.

- So, write git config core.autocrlf in the working directory to check the currently used value and
  - git config --system core.autocrlf false            # per-system solution
  - `git config --global core.autocrlf false`            # per-user solution
  - git config --local core.autocrlf false              # per-project solution

- Warnings
  - git config settings can be overridden by gitattributes settings.
  - crlf -> lf conversion only happens when adding new files, crlf files already existing in the repo aren't affected.

### renormalize

Since 2018, git can --renormalize repo fixing the existing line endings as required.
  
## Further Reading

- [gitattributes templates](https://github.com/alexkaratarakis/gitattributes)
- [github guide to line endings](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings)
- [dos2unix brew install](https://formulae.brew.sh/formula/dos2unix)
- [dos2unix source](https://waterlan.home.xs4all.nl/dos2unix.html)
- [how autocrlf works](https://stackoverflow.com/questions/1967370/git-replacing-lf-with-crlf)
- ["Mind the End of Your Line" further reading](https://adaptivepatchwork.com/2012/03/01/mind-the-end-of-your-line/)

