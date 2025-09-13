# Package management

apt & Homebrew

## Package Management on Debian-based Distributions

Trước khi tải package nào mới về thì phải update -> upgrage.

`sudo apt update` : update repository indexes (packet indexes). **Always** run this command before running `apt upgrade`

`sudo apt upgrade`: upgrade already installed packages to the latest version. Will not remove any packages or install any new packages.\
`sudo apt dist-upgrade` same as `upgrade` but will remove and install if it is required. You should run this after running `upgrade`

`sudo reboot` reboot the system (some updates will recommend you to reboot)

`apt search <package name>` search for a particular package by name or description. Searching does not write any changes to the system, so we don't need to use `sudo`.

`sudo apt install <package name>` install package

`sudo apt remove <package name>` remove package
`sudo apt autoremove` remove dependency packages that are no longer required

`apt get` is a derivative command inside `apt`

[Package Management on Fedora and CentOS (dnf and yum)](https://www.youtube.com/watch?v=-mvcOpMBlfs&list=PLT98CRl2KxKHaKA9-4_I38sLzK134p4GJ&index=21)

Find CPU architecture: `uname -m`. Máy dell là x86_64. Máy mac là `arm64`

[check ubuntu version](https://askubuntu.com/questions/686239/how-do-i-check-the-version-of-ubuntu-i-am-running), code name.

[stack exchange](https://unix.stackexchange.com/questions/590027/what-does-the-error-line-prefix-w-mean-like-from-apt-get-or-other-similar) meaning of error line prefixes: E, W, N

apt: Advanced Package Tool

## Install from binary

k

## Homebrew notes

On Apple silicon, Homebrew installs files into the `/opt/homebrew/` folder, which is not part of the default shell `$PATH`. You'll need to configure your shell environment so Homebrew packages are found and take priority over pre-installed tools.

Show a list of common commands `brew`
Show help `man brew`

`brew install <formula>` install a formula

To see all the packages in your local environment: `brew list`. It will show dependencies as well as packages you've installed.
You can also see a diagram of packages and dependencies `brew deps --tree --installed`

## View & install packges

The `brew tap` command adds more repositories to the list of formulae that Homebrew tracks, updates, and installs from. By default, `tap` assumes that the repositories come from GitHub, but the command isn’t limited to any one location.

Homebrew has its own terminology, referring to installed CLI packages as "kegs" and installed GUI packages as "casks." It's far more likely you'll use Homebrew to install a CLI package, since that is what Homebrew is known for. Casks were recently introduced to allow scripted installation of macOS GUI applications without the manual steps of downloading a .dmg file and dragging it to the Application folder.

`brew list` or `brew deps --tree --installed`

`brew search <package>` to see if a package is available

After installing a package, use `brew list <package>` to verify that it has been installed. You'll see all the installed files.
You can also see a list of dependencies for the package, if there are any: `brew deps <package>`

`apt list --manual-installed=true`

## Update

The command `brew outdated` will list alsl installed packages that have a newer version available. After running `brew outdated`, you can run `brew upgrade` to upgrade all packages or `brew upgrade <package>` to upgrade a specific package.

The command `brew update` is for updating Homebrew itself, as well as the Homebrew core packages that are installed by default.

The command `brew autoremove` will remove all unused dependencies remaining in the environment. If you've removed packages, it is likely that abandoned dependencies were left on disk if you didn't run `brew autoremove` immediately.

Homebrew maintains a cache of downloaded packages so repeated installation goes faster. The command `brew cleanup` will remove outdated download files from the cache, as well as old versions of installed packages. By default, `brew cleanup` only removes files more than 120 days old. Force a more recent cleanup with `--prune=all`.

## Uninstall

Here are the steps to uninstall a package with Homebrew on a Mac.

`brew list <package>` to see an installed package
`brew uninstall <package>` to remove a package
`brew list <package>` to verify removal
`brew autoremove` to remove unused dependencies

`brew cleanup` Remove stale lock files and outdated downloads for all formulae and casks, and remove old versions of installed formulae. If arguments are specified, only do this for the given formulae and casks. Removes all downloads more than 120 days old.

## References

[Terminology explained](https://mac.install.guide/homebrew/)

