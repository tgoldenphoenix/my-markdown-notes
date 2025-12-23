# Network Labs

## Packet Sniffing Notes

Wireshark captures link-layer frames as shown in Figure 1, but uses the generic term “packet” to refer to link-layer frames, network-layer datagrams, transport-layer segments, and application-layer messages, so we’ll use the less-precise “packet” term here to go along with Wireshark convention).

## Cisco Packet Tracer

Packet Tracer is a network simulator. It allows you to drag and drop virtual routers, switches, and PCs onto a canvas to build a network from scratch.

## Accessing the CLI of a Cisco device

For the CCNA exam, you must be familiar with the CLI of Cisco routers and switches running Cisco IOS.

In Cisco Packet Tracer, you can simply click on a device’s icon to access the CLI.

To configure Cisco devices (switch & router), you first have to connect your computer to the device to access the CLI. There are two main methods to do so:

- Connect a PC/laptop to the `console port` of the device with a console cable.
- Connect to the device over the network using a protocol like Telnet or Secure Shell (SSH).

The console port is a physical port that allows you to connect a computer directly to the device (as opposed to connecting via the network infrastructure).

Console ports cannot be used to communicate over the network. They are dedicated to configuring the device via the CLI.

## Cisco IOS CLI

Both Cisco routers and switches run the same operating system: Cisco IOS.

### The EXEC modes

`User EXEC mode` is the least-privileged mode in the Cisco IOS command hierarchy; it allows you to enter some basic commands to view information about the device’s configuration and status. However, it does not allow you to do anything intrusive like make any changes to the device’s configuration, restart the device, etc.

There are a variety of `show` commands that you will become familiar with throughout this book. Learning the available show commands and how to interpret their output is a major part of studying for the CCNA.

`show clock`

To access `privileged EXEC mode`, use the `enable` command.

`reload` restarts the device

Although privileged EXEC mode is more powerful than user EXEC mode, both modes are limited in that they do not allow you to make changes to the device’s configuration. The EXEC modes only allow you to view the device’s status and configuration, as well as execute operational commands to perform actions like restart the device, save the configuration, move and delete files, etc.

### Global configuration mode

use the `configure terminal` command from privileged EXEC mode

From global configuration mode, you can configure various features like the device’s hostname and passwords. From this mode, you can also access the other configuration modes

`hostname R1` change host name to `R1` (router)

If you want to undo a configuration command, you can use `no` in front of the command. For example, after the `hostname R1` command, `no hostname R1` would remove the command and revert the device’s hostname to the default of `Router`.

To return from global configuration mode to privileged EXEC mode, there are a few options. The `end` command, the `Ctrl-C` keyboard shortcut, and the `Ctrl-Z` keyboard shortcut will return you to privileged EXEC mode from global configuration mode or any other configuration mode. The `exit` command will return you to privileged EXEC mode from global configuration mode. However, if you’re in another configuration mode, it will return you to global configuration mode.

NOTE: If you use the `Ctrl-Z` shortcut in the middle of typing a command, the device will execute the typed command before returning to privileged EXEC mode; it’s equivalent to pressing Enter and then issuing end. Be careful! Ctrl-C does not do this; it will just return you to privileged EXEC mode.

---

Configuration modes such as global configuration mode allow you to configure the device, but EXEC mode commands like `show` do not work. However, the `do` command allows you to use EXEC mode commands from a configuration mode, so you don’t have to return to privileged EXEC mode. This can speed up your workflow when you are configuring a device but also want to use show commands to check its status. 

```bash
R1(config)# show clock
              ^
% Invalid input detected at '^' marker.
R1(config)# do show clock
*03:06:22.892 UTC Fri Feb 10 2023
```

---

Auto-completion with Tab

- Thậm chí không cần dùng tab, có thể executing partial commands if there is only one option beginning with the currently typed characters:
  * `en` = `enable`
  * `conf t` = `configure terminal`

### Getting help

A question mark (`?`) can be used for help in the Cisco IOS CLI in a few ways:

- To list the available commands in the current EXEC or configuration mode
- To list the keywords available for a command: `show ?`, `show clock ?`
- To list the possible completions of a partially typed command or keyword: `e?` list multiple commands that begin with e.

## Common Commands

The command to view a Cisco switch’s MAC address table is `show mac address-table` (in user EXEC or privileged EXEC mode).

## IOS configuration files

Cisco IOS devices make use of two different text files that store the device’s configurations: running-config and startup-config. The two files are each stored in different hardware memory and serve different purposes. You can view each configuration file with the `show running-config` and `show startup-config` commands.

The configurations in the running-config file determine the current operations of the device. When you enter a configuration command in the CLI, you are modifying the running-config file. Changes take effect instantly; as shown previously, after the `hostname` command is executed, the hostname of the device changes immediately.

The running config file is stored in random-access memory (RAM). It is important to note that the contents of RAM are lost when the device is powered off or restarted; therefore, changes to running-config are lost in either event. To save configuration changes so they persist even if the device is powered off or restarted, the startup-config file is used.

The configurations in startup-config do not determine the current operations of the device. Rather, the startup-config is the configuration file that is loaded by the device when it boots up—for example, after being powered on or restarted. The contents of the startup-config file are copied to the running-config file in RAM when the device boots up.

The startup-config file is stored in a special type of RAM called `nonvolatile RAM (NVRAM)`. The contents of NVRAM are kept even when the device is powered off or restarted, so to save changes made to the running-config file, the contents must be copied to startup-config. Otherwise, the device will have a `factory-default` configuration every time it boots up.

There are a few different commands (entered in privileged EXEC mode) that can be used to copy the contents of the running-config file to the startup-config file. The effect of each of these commands is the same, so it doesn’t matter which one you use:

- `write`
- `write memory`
- `copy running-config startup-config`

If you want to return a device to its factory-default configuration, you can erase startup-config and then restart the device with the reload command. Just as with saving the configuration, there are a few different commands you can use to delete startup-config:

- `write erase`
- `erase nvram:`
- `erase startup-config`

## Port names on Cisco devices

Ports on Cisco devices have a name indicating their maximum supported speed (Ethernet = 10 Mbps, FastEthernet = 100 Mbps, GigabitEthernet = 1 Gbps, TenGigabitEthernet = 10 Gbps), followed by one to three numbers. How many numbers are used depends on the model of the device.

In this book, I will use a two-number system (X/Y), where the first number is the slot on the device, and the second number is the port number within that slot. A slot is a group of ports on a network device. In many cases, the ports in a slot are modular, meaning you can insert modules with different kinds of ports depending on your needs. Additionally, I will shorten the names to use the first letter only: E = Ethernet, F = FastEthernet, G = GigabitEthernet, T = TenGigabitEthernet.

Furthermore, port numbers on physical Cisco switches start from 1 (G0/1, G0/2, G0/3, etc). However, for most examples in this book, I will use virtual devices running in Cisco’s emulation software CML (Cisco Modeling Labs), in which port numbers start from 0 (G0/0, G0/1, G0/2, etc).

Cisco abbreviates GigabitEthernet ports as “GiX/X,” not “GX/X.”
