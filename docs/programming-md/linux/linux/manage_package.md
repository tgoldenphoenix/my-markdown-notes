# Package Management

If `XDG_CONFIG_HOME` is not set, the default is `~`

## Ubuntu `apt`

- `apt`: Advanced Package Tool
- `apt` is newer, `apt-get` is older, legacy. You should use `apt`.

```bash
sudo apt update
sudo apt upgrade
```

- Chạy `update` trước `upgrage`.
- `sudo apt update` update repository indexes (packet indexes), ensuring that `APT` is aware of all the most recent packages and versions available.
- `sudo apt upgrade` installs the software (applies the actual updates)

`apt upgrade -y` will download and install any relevant upgrades. `-y` will automatically answer `Yes` when asked to confirm the operation.

The `update` command connect to the URI of the **official repository** at `https://archive.ubuntu.com/ubuntu/` and downloads the latest package index lists (the "catalogues") to find out what new versions of software are available.

`sudo apt upgrade`: upgrade already installed packages to the latest version. Will not remove any packages or install any new packages.

`sudo reboot` reboot the system (some updates will recommend you to reboot)

`sudo apt install <package name>` install package

- `sudo apt remove <package name>` remove package
- `sudo apt autoremove` remove dependency packages that are no longer required

Find CPU architecture: `uname -m` or `lscpu`. Máy dell là x86_64. Máy mac là `arm64`

[check ubuntu version](https://askubuntu.com/questions/686239/how-do-i-check-the-version-of-ubuntu-i-am-running), code name.

[stack exchange](https://unix.stackexchange.com/questions/590027/what-does-the-error-line-prefix-w-mean-like-from-apt-get-or-other-similar) meaning of error line prefixes: E, W, N

---

`apt search <package name>` search for a particular package by name or description. Searching does not write any changes to the system, so we don't need to use `sudo`.

APT systems let you directly search for available packages using apt search. This example searches for packages that might help you monitor your system health and then uses apt show to display full package information:

```bash
apt search sensors
apt show lm-sensors
```

version

```bash
❯ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.5 LTS
Release:        24.04
Codename:       noble
```

binary install thủ công thì `mv` vào `/usr/local/bin`. Còn install thông qua package manager thì nó nằm trong `/usr/bin`

## Add a new repo & install package with `dnf`

When need to add the virtualbox repository to Yum.

- In the Red Hat/Fedora/CentOS ecosystem:
  - `rmp`, giống `dpkg`, handles installing/verifying individual `.rpm` files (giống `.deb` files).
  - `yum`Fetches packages from repositories; resolves dependencies. Handle Dependency Management (slow).
  - `dnf` is the modern version of `yum`, faster
  - The rpm command was the first tool to manage RPMs. Later, yum was added to enhance the RPM facility, and now dnf is poised to eventually replace yum.

From the previous chapter, you’ll remember that third-party software configuration files are often kept within the /etc/ directory hierarchy, and, in that respect, yum/DNF is no different. Repository information is kept in /etc/yum.repos.d/, so you should change to that directory. From there, you’ll use the wget program (usually installed by default) to download the .repo file. Here’s how to do all that:

```bash
$ cd /etc/yum.repos.d/
# wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
[sudo] password for anhao:
--2025-12-04 10:00:16--  http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
Resolving download.virtualbox.org (download.virtualbox.org)... 23.53.212.118
Connecting to download.virtualbox.org (download.virtualbox.org)|23.53.212.118|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 246 [text/plain]
Saving to: ‘virtualbox.repo’

virtualbox.repo                   100%[===========================================================>]     246  --.-KB/s    in 0s

2025-12-04 10:00:16 (9.44 MB/s) - ‘virtualbox.repo’ saved [246/246]
```

Having the `.repo` file in the right directory won’t do much until you tell RPM what’s changed. You do that by running update. The update command also checks the local repository index against its online counterparts to see whether there’s anything new you’ll want to know about. No matter what manager you’re using, it’s always a good idea to update the repo information before installing new software:

```bash
# dnf update

# dnf install VirtualBox-5.1
```

## Install manually from binary archive

- `.tar.gz`
  - .tar = tape archive, a single bundle of files (like .zip).
  - `.gz` = `gzip` compression, to make it smaller.
- So `.tar.gz` ≈ a compressed archive similar to a .zip file on Windows.
- Require `tar` & `gzip`.

You can extract it using: `mkdir - /target/dir && tar -xvzf filename.tar.gz -C /target/dir`

- The meaning of `-xvzf` are:
  - `-x --extract` = extract files from an archive
  - `-v, --verbose` = verbosely list files processed
  - `-z, --gzip` for gzipped files eg. `tar.gz` (for `.tar.xz` use `-J` (uppercase) instead.)
  - `-f` (`--file`) the next argument is the name of the archive
  - The `-C` (change directory) flag tells tar to "jump" to a different folder before starting the extraction.

The `tar` command comes pre-install on Linux.

- There are two kinds of `.tar.gz` archives you might see:
  - `Source` `.tar.gz` Contains source code; you must compile it yourself (using `make`, `cmake`, `gcc`, etc.)
  - `Binary .tar.gz` Contains already compiled executable files — ready to run directly on your system
- So a `binary tar.gz` saves you the build step — it’s prebuilt for your architecture (e.g., x86_64 Linux).

Tải `.tar.gz` về > extract ra > move to `/usr/local/` > add to PATH.

- Binary tarballs are often used when:
  - The software is not available in your package manager (e.g., apt, dnf, pacman).
  - You want a specific version or portable installation (no installation required).

There are 2 file extensions because the files are first rolled into a tar file, which combines multiple files and folders into one while keeping all the properties, but does not do any compression. Then the compression algorithm is ran on the combined file, resulting in a compressed archive. You can do this with other compressors as well, like bzip2 (.tar.bz2), xz (.tar.xz) and zstd (.tar.zst). GZip isn't the fastest or most efficient, but it's available everywhere, so it's the standard for compressing stuff on Linux.

### Where to store binary and Scripts?

Move the downloaded binary to `/opt`.

Create symbolic link to `/usr/local/bin`: `sudo ln -s /opt/extracted-app-name/bin/program-executable /usr/local/bin/program-name`

tạo symlink file binary nvim vào `usr/bin`: `ln -s /opt/nvim-linux-x86_64/bin/nvim /usr/bin/`. Không add thêm vào `$PATH` khiến nó dài ra.

Hoặc đem binary bỏ vô `/usr/local/bin/` luôn.

---

```bash
# lên official website tải file nvim-linux-x86_64.tar.gz về
# extract
tar xvf nvim-linux-x86_64.tar.gz
# copy binary
sudo cp -r nvim-linux-x86_64/* /usr/local/
```

`sudo cp -r nvim-linux-x86_64/* /usr/local/` merges directory contents cleanly rather than replacing entire folders.

When you install another application using the same structure (e.g., copying a tool like `ripgrep` or `fd` into `/usr/local/`):

- `bin/`: Linux keeps files side-by-side in `/usr/local/bin/`. Installing another app puts its executable (e.g., `/usr/local/bin/rg`) right next to `/usr/local/bin/nvim`.
- `lib/`: Files are stored in separate application subdirectories (e.g., `/usr/local/lib/nvim/` vs. `/usr/local/lib/another_app/`).
- `share/`: Application resources are similarly segregated (e.g., `/usr/local/share/nvim/` vs. `/usr/local/share/another_app/`).

---

- If you want to add your own commands or shell scripts, place them in the bin directory in your home directory (such as `/home/chris/bin` for the user named `chris`).
- To make commands available to all users, add them to `/usr/local/bin`.

Unlike some other operating systems, Linux does not, by default, check the current directory for an executable **before** searching the path. It immediately begins searching the path, and executables in the current directory are run only if they are in the PATH variable or you give their absolute (such as `/home/chris/scriptx.sh`) or relative (for example,  ./script x.sh) location.

The path directory order is important. Directories are checked from left to right. So, in this example, if there is a command called foo located in both the /usr/bin and /bin direc-tories, the one in /usr/bin is executed. To have the other foo command run, you either type the full path to the command or change your PATH variable.

`which <command>` locate the executable file associated with the given command.

`type` giống `which`

```bash
$ type bash
bash is /bin/bash

❯ type which
which is a shell builtin

❯ type case
case is a reserved word
```

---

If a command is not in your PATH variable, you can use the `locate` command to try to
find it. Using locate, you can search any part of the system that is accessible to you. (Some files are only accessible to the root user.) For example, if you wanted to find the loca-tion of the chage command, you could enter the following:

```bash
$ locate chage
/usr/bin/chage
/usr/sbin/lchage
/usr/share/man/fr/man1/chage.1.gz
/usr/share/man/it/man1/chage.1.gz
/usr/share/man/ja/man1/chage.1.gz
/usr/share/man/man1/chage.1.gz
/usr/share/man/man1/lchage.1.gz
/usr/share/man/pl/man1/chage.1.gz
/usr/share/man/ru/man1/chage.1.gz
/usr/share/man/sv/man1/chage.1.gz
/usr/share/man/tr/man1/chage.1.gz
```

Notice that locate not only found the chage command, it also found the lchage command and a variety of man pages associated with chage for different languages. The locate command looks all over your filesystem, not just in directories that contain com-mands. (If locate does not find files recently added to your system, run `updatedb` as root to update the locate database.)

`which -a ls` show that the `ls` command is in the `/bin` directory (show full path)

`whereis aws`

- `which`:
  - f
- `whereis`

### Binary ZIP archive

A binary `.zip` archive is simply a compressed ZIP file that contains precompiled executable files (binaries) instead of source code.

`.zip` is common on **Windows**.

`unzip file.zip`

The `unzip` command is pre-install on Mac & linux.

### `curl`

The `--remote-name` or `-O` option will download & save the file using the name provided in the URL.

```bash
$ curl -O https://starship.rs/install.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 14180    0 14180    0     0  20380      0 --:--:-- --:--:-- --:--:-- 20402
```

`curl -sS https://starship.rs/install.sh | sh`

- `curl -sS`: downloads the install script from the official Starship site quietly (`-s`) but still shows errors (-`S`).
- `https://starship.rs/install.sh`: is the official installer URL.
- `| sh`: pipes the downloaded script to the shell to execute it.

Starship nằm trong `usr/local/bin`

```bash
Finished. Restart your shell or reload config file.
   source ~/.bashrc  # bash
   source /home/anhao/.config/zsh/.zshrc   # zsh

Use uninstall script to remove fzf.
```

## Working with `dpkg`

Download `.deb` or `.rpm` file on the website.

Once you download the file, you can install it from the command line using `dpkg` command. Use the `-i` flag (for install). You’ll need to make sure that you’re running the dpkg command from the directory where the `skypeforlinux-64` file is located. This example assumes that you saved the package to the Downloads directory in your user account:

```bash
$ cd /home/<username>/Downloads
# dpkg -i skypeforlinux-64.deb
```

The `dpkg` command should take care of dependencies for you. But if it doesn’t, its output will usually give you enough information to know what’s going on.

`dpkg` (Debian Package) is the core package handler; `apt` (Advanced Package Tool) is the smart tool you use every day that relies on dpkg to do the actual work.

`dpgk` is used to install `.deb` files.

Running `dpkg` with the `-s` flag and the name of a package returns the current installed and update status. If the package is already installed (as is true for this `gedit` example), the output will look something like this:

```bash
$ dpkg -s gedit
Package: gedit
Status: install ok installed
Priority: optional
Section: gnome
Installed-Size: 1732
Maintainer: Ubuntu Desktop Team <ubuntu-desktop@
  lists.ubuntu.com>
Architecture: amd64
Version: 3.18.3-0ubuntu4
Replaces: gedit-common (<< 3.18.1-1ubuntu1)
Depends: python3:any (>= 3.3.2-2~), libatk1.0-0
    (>= 1.12.4)
[...]
```

## Ubuntu snap

In Ubuntu, `Snap` is a modern, universal package management system developed by Canonical (the company behind Ubuntu). It is designed to make software installation easier, more secure, and more consistent across different versions of Linux.

## Fedora

Fedora is the upstream, community-driven project that acts as the testing ground for Red Hat Enterprise Linux (RHEL).

`rpm` (Red Hat Package Manager)

## Homebrew Notes

On Apple silicon, Homebrew installs files into the `/opt/homebrew/` folder, which is not part of the default shell `$PATH`. You'll need to configure your shell environment so Homebrew packages are found and take priority over pre-installed tools.

Show a list of common commands `brew`  
Show help `man brew`

`brew install <formula>` install a formula

To see all the packages in your local environment: `brew list`. It will show dependencies as well as packages you've installed.

You can also see a diagram of packages and dependencies `brew deps --tree --installed`

## View Installed Packges

The `brew tap` command adds more repositories to the list of formulae that Homebrew tracks, updates, and installs from. By default, `tap` assumes that the repositories come from GitHub, but the command isn’t limited to any one location.

Homebrew has its own terminology, referring to installed CLI packages as `kegs` and installed GUI packages as `casks`. It's far more likely you'll use Homebrew to install a CLI package, since that is what Homebrew is known for. Casks were recently introduced to allow scripted installation of macOS GUI applications without the manual steps of downloading a .dmg file and dragging it to the Application folder.

`brew list` or `brew deps --tree --installed`

`brew search <package>` to see if a package is available

After installing a package, use `brew list <package>` to verify that it has been installed. You'll see all the installed files.
You can also see a list of dependencies for the package, if there are any: `brew deps <package>`

---

Ubuntu

```bash
apt list --manual-installed=true

apt-mark showmanual
```

`apt list --installed` show một đống

`apt list --installed | less` more manageable output

`apt list --installed | grep <package_name>`  search for a specific package

`apt list --manual-installed`

## Update Packages

The command `brew outdated` will list all installed packages that have a newer version available. After running `brew outdated`, you can run `brew upgrade` to upgrade all packages or `brew upgrade <package>` to upgrade a specific package.

The command `brew update` is for updating Homebrew itself, as well as the Homebrew core packages that are installed by default.

The command `brew autoremove` will remove all unused dependencies remaining in the environment. If you've removed packages, it is likely that abandoned dependencies were left on disk if you didn't run `brew autoremove` immediately.

Homebrew maintains a cache of downloaded packages so repeated installation goes faster. The command `brew cleanup` will remove outdated download files from the cache, as well as old versions of installed packages. By default, `brew cleanup` only removes files more than 120 days old. Force a more recent cleanup with `--prune=all`.

## Environment Variables & Viewing Machine specs

`echo $0` to know which shell you are using

On Linux

On MacOS

Use the `printenv` command to display a list of currently set environment variables. Trong này sẽ có `$PATH`, `$HOME` và nhiều cái khác.

Hiển thị one environment variable: `echo $[variable name]`

`$PATH` environment variable stores a list of directories with executable files, and thus saves the user a lot of typing and memorizing locations of commands.

If you want to display the complete list of shell variables, use the `set` command. The output is very long.

`$HOME`: /Users/anhao

The value you assign to a **temporary** environment variable only lasts until you close the terminal session. This is useful for variables you need to use for one session only or to avoid typing the same value multiple times.

Assign a temporary environment variable with the `export` command. The export command also allows you to add new values to existing environment variables with the `:$` syntax.

**Permanent** environment variables are added to the `.bash_profile` file. Cũng dùng `export` command. Execute the new `.bash_profile` by either restarting the terminal window or using `source ~/.bash-profile`

Use the`unset` command to remove an environment variable.

The primary use of the `eval` command  is to interpret and execute dynamic or complex commands stored in strings or variables. This allows you to generate and run commands dynamically.

References

[how to add folder to PATH](https://phoenixnap.com/kb/linux-add-to-path) by PHOENIXNAP very informative

## Archiving Files and file systems using `tar`

In computing, `tar` is a computer software utility for collecting many files into one archive file, often referred to as a **tarball**, for distribution or backup purposes.  
The name is derived from **tape archive**, as it was originally developed to write data to sequential I/O devices with no file system of their own, such as devices that use **magnetic tape**.

- To successfully create your archive, there are three things that will have to happen:
  - Find and identify the files you want to include.
  - Identify the location on a storage drive that you want your archive to use.
  - Add your files to an archive, and save it to its storage location.

Want to knock off all three steps in one go? Use `tar`.

This example copies all the files and directories within and below the current work directory and builds an archive file that I’ve cleverly named `archivename.tar`. Here I use three arguments after the `tar` command:

- the `c` tells tar to create a new archive (compress)
- `v` sets the screen output to verbose so I’ll get updates
- and `f` points to the filename I’d like the archive to get

```bash
$ tar cvf archivename.tar *
file1 # The verbose argument (v) lists the names of all the files added to the archive.
file2
file3
```

The `tar` command will never move or delete any of the original directories and files you feed it; it only makes archived copies. You should also note that using a dot (.) instead of an asterisk (*) in the previous command will include even hidden files (whose filenames begin with a dot) in the archive.

The `.tar` filename extension isn’t necessary, but it’s always a good idea to clearly communicate the purpose of a file in as many ways as possible.

---

To extract the archive, run the tar command against the name of the archive, but this time with the argument `x` (for extract) rather than `c`:

```bash
$ tar xvf stuff.tar
```

**WARNING**: Extracting an archive overwrites any files with the same names in the current directory without warning. Here, that’s fine, but that won’t normally be the case.

---

You won’t always want to include all the files within a directory tree in your archive. Suppose you’ve produced some videos, but the originals are currently kept in directories along with all kinds of graphic, audio, and text files (containing your notes). The only files you need to back up are the final video clips using the .mp4 filename extension. Here’s how to do that:

`$ tar cvf archivename.tar *.mp4`

That’s excellent. But those video files are enormous. Wouldn’t it be nice to make that archive a bit smaller using compression? Say no more! Just run the previous command with the `z` (zip) argument. That will tell the `gzip` program to compress the archive. If you want to follow convention, you can also add a `.gz` extension in addition to the `.tar` that’s already there. Remember: clarity. Here’s how that would play out:

`$ tar czvf archivename.tar.gz *.mp4`

You may notice that the `.tar.gz` file isn’t all that much smaller than the .tar file, perhaps 10% or so. What’s with that? Well, the `.mp4` file format is itself compressed, so there’s a lot less room for gzip to do its stuff.

---

As tar is fully aware of its Linux environment, you can use it to select files and directories that live outside your current working directory. This example adds all the .mp4 files in the `/home/myuser/Videos/` directory:

`$ tar czvf archivename.tar.gz /home/myuser/Videos/*.mp4`

---

Because archive files can get big, it might sometimes make sense to break them down into multiple smaller files, transfer them to their new home, and then re-create the original file at the other end. The split tool is made for this purpose.

In this example, `-b` tells Linux to split the `archivename.tar.gz` file into 1 GB-sized parts; archivename is any name you’d like to give the file. The operation then names each of the parts—`archivename.tar.gz.partaa`, `archivename.tar.gz.partab`, `archivename .tar.gz.partac`, and so on:

`$ split -b 1G archivename.tar.gz "archivename.tar.gz.part"`

On the other side, you re-create the archive by reading each of the parts in sequence (`cat archivename.tar.gz.part*`), then redirect the output to a new file called `archivename.tar.gz`:

`$ cat archivename.tar.gz.part* > archivename.tar.gz`

### Streaming file system archives

```bash
# tar czvf - --one-file-system / /usr /var \
  --exclude=/home/andy/ | ssh username@10.0.3.141 \
  "cat > /home/username/workstation-backup-Apr-10.tar.gz"
```

```bash
$ tar czvf - importantstuff/ | ssh username@10.0.3.141 \
  "cat > /home/username/myfiles.tar.gz"
```

Let me explain that example. Rather than entering the archive name right after the command arguments (the way you’ve done until now), I used a dash (`czvf -`). The dash **outputs data to standard output**. It lets you push the archive filename details back to the end of the command and tells tar to expect the source content for the archive instead. I then piped (|) the unnamed, compressed archive to an ssh login on a remote server where I was asked for my password. The command enclosed in quotation marks then executed cat against the archive data stream, which wrote the stream contents to a file called `myfiles.tar.gz` in my home directory on the remote host.

One advantage of generating archives this way is that you avoid the overhead of a middle step. There’s no need to even temporarily save a copy of the archive on the local machine. Imagine backing up an installation that fills 110 GB of its 128 GB of available space. Where would the archive go?

## Computer Processor Architectures

- Theo công thức chung`{software-name}_{version}_{Operating System}_{CPU architecture}.tar.gz`
  - `lazygit_0.56.0_darwin_arm64.tar.gz`
  - `lazygit_0.56.0_darwin_x86_64.tar.gz`
  - `lazygit_0.56.0_linux_32-bit.tar.gz`

Darwin is the open-source core operating system that underlies Apple's operating systems, including macOS, iOS, iPadOS, watchOS, and tvOS.

---

CPU Processor architectures

- `ARMv6` is a legacy, 32-bit architecture used in older, low-power embedded systems.
- `ARM64` and `AArch64` are the same thing. `AArch64` is the official name for the 64-bit ARM architecture, but some people prefer to call it `ARM64` as a continuation of 32-bit ARM.)

`x86-64` (also known as `x64`, `x86_64`, `AMD64`, and `Intel 64`) is a 64-bit extension of the `x86` instruction set.

---

The term `x86` refers to the entire family of processor architectures developed by Intel (and later AMD). The terms `32-bit` and `64-bit` refer to the width of the registers and memory addresses within that architecture.

Today, when you see `x86`, it almost always refers to the 32-bit version of this architecture.

Linux, like other `x86-based` operating systems, comes in both 64-bit and 32-bit versions. The vast majority of computers manufactured and sold over the past decade use the faster 64-bit architecture. Because there’s still older or development-oriented hardware out there, you’ll sometimes need to run 32-bit, and you’ll want the software you install to work with it.

You can check for yourself by running `arch` from the command line. Unless you know you’re running on older hardware (something Linux does particularly well, by the way), you’re safe assuming that you’re a 64-bit kind of person.

The arch command in Linux is a simple utility used to display the architecture of the machine's processor (the CPU).

It is functionally equivalent to running the command `uname -m`.

## On Windows

A .`zip` file is just a container of files, while an `.msi` file contains a database of instructions used by the Windows operating system to perform a complex installation process.

- `zip` is Cross-Platform (Windows, Mac, Linux, Web).
- `msi` is Windows-specific (Relies on Windows Installer service).

- `nvim-win64.zip` (or nvim-win-arm64.zip for ARM)
- `nvim-win64.msi` (or nvim-win-arm64.msi for ARM)

An `MSI` (Microsoft Software Installer) file is a package that contains all the necessary information and instructions for installing, maintaining, and removing software on Windows systems

## Uninstall

Here are the steps to uninstall a package with Homebrew on a Mac.

- `brew list <package>` to see an installed package
- `brew uninstall <package>` to remove a package
- `brew list <package>` to verify removal
- `brew autoremove` to remove unused dependencies

`brew cleanup` Remove stale lock files and outdated downloads for all formulae and casks, and remove old versions of installed formulae. If arguments are specified, only do this for the given formulae and casks. Removes all downloads more than 120 days old.

## Give permission

`chmod u+x nvim-linux-arm64.appimage && ./nvim-linux-arm64.appimage`

Đầu tiên phải thêm quyền bằng `chmod`, sau đó thì move vào `PATH`. Có 2 bước đó.

```bash
chmod +x virtualmachine.sh
./virtualmachine.sh
```

## References

[Terminology explained](https://mac.install.guide/homebrew/)
