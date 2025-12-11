# Linux Desktop Environment

## Linux Desktop Technology

There are fully featured GNOME or KDE desktop envi-ronments or lightweight desktops such as LXDE or Xfce. There are even simpler standalone window managers.

Nearly every major Linux distribution that offers desktop interfaces is based on the `X Window System` originally from the X.Org Foundation (<http://www.x.org>).  
The X Window System provides a framework on which different types of desktop environments or simple window managers can be built.  
A replacement for X.Org called Wayland (<http://wayland.freedesktop.org>) is being developed. Although Wayland is the default X server for Fedora now, you can still choose X.Org instead.

The `X Server` is the foundational core component of the X Window System (also known as X11 or simply "X"), which provides the basic framework for graphical user interfaces (GUIs) on Linux and other Unix-like operating systems.

The `X Window System` (sometimes simply called `X`) was created before Linux existed, and 
it even predates Microsoft Windows. It was built to be a lightweight, networked desktop framework.

`X` works in sort of a backward client/server model. The X server runs on the local system, providing an interface to your screen, mouse, and keyboard. X clients (such as word proces-sors, music players, and image viewers) can be launched from the local system or from any system on your network to which the X server gives permission to do so.

---

`X` itself provides a plain gray background and a simple “X” mouse cursor. There are no menus, panels, or icons on a plain X screen. If you were to launch an X client (such as a ter-minal window or word processor), it would appear on the X display with no border around it to move, minimize, or close the window. Those features are added by a `window manager`.

A window manager adds the capability to manage the windows on your desktop and often provides menus for launching applications and otherwise working with the desktop. A full-blown `desktop environment` includes a window manager, but it also adds menus, panels, and usually an `application programming interface` that is used to create applications that play well together.

- Because Linux desktop environments are not required to run a Linux system, a Linux system may have been installed without a desktop. It might offer only a plain-text, command-line interface. You can choose to add a desktop later. After it is installed, you can choose whether to start up the desktop when your computer boots or start it as needed.
- For a very lightweight Linux system, such as one meant to run on less powerful computers, you can choose an efficient, though less feature-rich, window manager (such as twm or fluxbox) or a lightweight desktop environment (such as LXDE or Xfce).
- For more robust computers, you can choose more powerful desktop environments (such as `GNOME` and `KDE`) that can do things such as watch for events to happen (such as inserting a USB flash drive) and respond to those events (such as opening a window to view the contents of the drive).
- You can have multiple desktop environments installed and you can choose which one to launch when you log in. In this way, different users on the same computer can use different desktop environments.

A `Desktop Environment (DE)` is a comprehensive package that includes and relies upon both the underlying `display server` (X Window System or Wayland) and a `Window Manager`, along with many other components.

- Many different `desktop environments` are available to choose from in Linux. Here are 
some examples:
  * `GNOME`: GNOME is the default desktop environment for Fedora, Red Hat Enterprise Linux, and many others. Think of it as a professional desktop environment focusing on **stability more** than fancy effects.
  * `K Desktop Environment: KDE` is probably the second most popular desktop environment for Linux. It has more bells and whistles than GNOME and offers more integrated applications. KDE is also available with Fedora, Ubuntu, and many other Linux systems. For RHEL 8, KDE was dropped from the distribution.
  * `Xfce`: The Xfce desktop was one of the first **lightweight** desktop environments. It is good to use on older or less powerful computers. It is available with Fedora, Ubuntu, and other Linux distributions.
  * `LXDE`: The `Lightweight X11 Desktop Environment` (LXDE) was designed to be a fast-performing, energy-saving desktop environment. Often, LXDE is used on **less-expensive devices** (such as netbook computers) and on live media (such as a live CD or live USB stick). It is the default desktop for the KNOPPIX live CD distribution. Although LXDE is not included with RHEL, you can try it with Fedora or Ubuntu.

GNOME was originally designed to resemble the MacOS desktop, while KDE was meant to emulate the Windows desktop environment.

An `X window manager` is a window manager that runs on top of the X Window System.

`i3` is a tiling window manager designed for X11, inspired by wmii and written in C. 