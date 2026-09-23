# WSL & Git Bash Notes

## Basics

WSL 2 will install your Linux distribution within a hidden folder on your `C:` drive by default. You can move it to another drive to free up space on C:`.

---

In `PowerShell` (not the Ubuntu terminal), right-click the icon, and choose `Run as administrator`.

```bash
# list of available Linux distros
wsl --list --online

# check for Linux kernal updates from Powershell
wsl --update

# list your Linux installations
wsl --list
```

---

Any Linux (bash) shell command can be run from a Windows Powershell or command-line terminal using wsl:

```bash
wsl <linux-command>
```

For example, `wsl ls -la` lists all files in a Windows folder.

binary install thủ công thì `mv` vào `/usr/local/bin`. Còn install thông qua package manager thì nó nằm trong `/usr/bin`

## Powershell Commands

In PowerShell, `mkdir` is an alias (or built-in function) for the cmdlet `New-Item -ItemType Directory`.

## Access Linux Files from Windows

Enter `\\wsl$\` in the File Explorer address bar.

Your installed Linux distros are listed, so you can access the Ubuntu root directory at `\\wsl$\Ubuntu`. Your personal Linux files are typically be stored at `\\wsl$\Ubuntu\home\<yourname>`.

It’s best to set this as the starting folder for the distro in Windows Terminal. Open the Settings, click a profile, then change the `Starting directory` option.

## Access the Windows File System through WSL

```bash
cd /mnt/c/Users
**OR**
cd /mnt/d/your_folder/your_folder
```

- `/mnt/c` == `C:\`
- `/mnt/c/Users/` = `C:\Users`

Your personal Users folder at `C:\Users\<yourname>` is available at `/mnt/c/Users/<yourname>`.

For ease of access, you can create a Linux symbolic link to any Windows folder from the terminal. For example, for `C:\projects\code\`:

```bash
cd ~
ln -s /mnt/c/projects/code/ code
```

A `code` folder will appear in your Linux home directory. Navigate to it using `cd ~/code` and you’ll actually be in `/mnt/c/projects/code/`, which maps directly to `C:\projects\code\`.

Accessing Windows files from Linux is considerably slower than using the native Linux file system. Where possible, create projects in the Linux file space, typically in your home folder (`/home/<yourname>/` or `~`).

### Access the Windows File System through Git Bash

kk

## Run Windows Applications from Linux

Through Windows WSL2, you can launch Windows applications like Notepad or `VS Code` directly from your Linux environment.

```bash
explorer.exe .

# edit .bashrc in Notepad
notepad.exe ~/.bashrc

# open the a specific project directory in VS Code
code ~/projects/mywebsite
```

## Install Applications

Always remember you’re running two operating systems. They may be tightly integrated, but there are situations when you want an application installed everywhere.

For example, Git is useful in both Windows and Linux. The Windows edition is installed by downloading an executable. It’s best to ensure it doesn’t convert line endings:

```bash
git config --global core.autocrlf input
```

Git on Ubuntu is installed using:

```bash
sudo apt update
sudo apt install git-all
```

Similarly, you may want to test Node.js applications on both Windows and Linux. Again, Windows has a runtime installer while `Node.js` runtime is installed on Ubuntu using commands such as:

```bash
sudo apt-get install build-essential
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

git, node and npm commands will now work in either OS. Be wary that they may be different versions.

```bash
my-markdown-notes on  main [!] via  v24.11.1
❯ uv self update
info: Checking for updates...
success: Upgraded uv from v0.9.13 to v0.12.18! https://github.com/astral-sh/uv/releases/tag/0.12.18
```

## Git Bash vs WSL

Git Bash on Windows comes pre-packaged with full Windows ports of standard GNU command-line utilities, including grep, awk, sed, `find`, `curl`, and `tar`.

A PostgreSQL server happily listening on Windows localhost:5432 was not automatically reachable from inside WSL

they solve two very different problems. Git Bash is a POSIX-emulation layer running on Windows, while WSL 2 runs a real Linux kernel inside a lightweight managed VM. Those architectural differences explain almost every strange behavior you will see.

## MINGW

kk

## Free up disk space on Window

k

