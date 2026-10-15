# Working with Logs, CSV

Read system logs

On nearly all modern Linux distributions, you can access all **system logs** through journalctl: 

`# journalctl`

As you’ll quickly see, running `journalctl` without any arguments will drown you in a torrent of data. You’ll need to find some way to filter for the information you’re after. Allow me to introduce you to grep:

`# journalctl | grep filename.php`

You can use grep in sequence to narrow your results further:

`# journalctl | grep filename.php | grep error`

In case you’d prefer to see only those lines that don’t contain the word error, you’d add -v (for inverted results): 

`# journalctl | grep filename.php | grep -v error`

`dmesg` display all messages from the kernel ring buffer.

`tail -f /var/log/syslog` The `-f, --follow` output appended data as the file grows (helpful)

`journalctl` print log entries from the systemd journal
`journalctl -u <unit name>` print the log of a specific systemd unit
`journalctl -fu <unit name>` follow log

For Linux systems that don’t use the systemd facility, the main utility for logging error and debugging messages is the `rsyslogd` daemon. (Some older Linux systems use `syslogd` and syslogd daemons.) Although you can still use rsyslogd with systemd systems, systemd has its own method of gathering and displaying messages called the systemd journal (`journalctl` command).

## Administrative log files and systemd journal

The primary command for viewing messages from the systemd journal is the `journalctl` command. The boot process, the kernel, and all systemd-managed services direct their status and error messages to the systemd journal

## Visidata Basics

`vd -f tsv filename.csv` => dùng tab làm delimeter nhưng vẫn đặt tên là `.csv`

On the command line, `vd` and `visidata` are synonymous. You can use either to launch VisiData.

`vd --version`

`b` toggle sidebar for helpful information

`Ctrl h` open help menu. You can navigate the menu either with your arrow keys or by pointing and clicking with your mouse. To run a command from the menu, press `Enter` or click a second time on the selected item.

`realpath data.csv` => get the full path of the file

---

- `gh` to leftmost column
- `gl` to rightmost column

- `zr + n`	Jump to row number n
- `zc + n`	Jump to column number n

Press `Control-g` to display your cursor’s row and column number at the bottom of VisiData’s interface.

---

`shift f` => frequency table of a column

### Sheets

VisiData’s “Sheets Sheet” lists all currently-open sheets and makes it easy to jump between sheets.

open the Sheets Sheet by pressing `Shift-S`.

rename a sheet: go to the Sheets Sheet, and navigate to the row representing the sheet you want to rename. Press `e` to go into editing mode, type the new name, and then press Enter to complete the renaming.

To flip back and forth between your current sheet and the previous one, press `Control-^`.

### rows & column

- `s` Select the current row
- `u` Unselect the current row
- `t` Toggle the current row between selected/unselected

---

To see all the columns in your dataset, press `Shift-C`. This will open the “Columns Sheet,” which lists each column and its attributes.

Rather than guess at your column’s data types, VisiData assumes that they’re all plain-old text.

If a column of really is just text, then great.

But if that column is a number or date, and you want to do any math-y operations on your column (e.g., sorting, summing, averaging, et cetera), you’ll have to specify its type.

To set a column’s type, navigate over to that column and press one of the following keys:

- `#`	Integer =>	0, -1, 5000000
- `%`	Float =>	0.5, -3.14, 23.45557
- `$`	Currency =>	$4.99, €20, ₹ 100 100
- `@`	Date =>	2018-04-06, April 6, 2018, 04/06/2018
- `~`	Text =>	anything!

To entirely remove a column’s assigned type, navigate to the column and type `z~`.

---

When you load a dataset, VisiData will try to choose reasonable widths for your columns. You can adjust them in a few ways:

- `_`	Expands the width of current column to fit text in all visible rows
- `g_`	Expands the width of all columns to fit text in all visible rows
- `-`	Hides the current column by setting its width to 0 (`b` toggle sidebar to see how to un-hide the column)
- `z-`	Shrinks the current column’s width in half

---

Sometimes you want to view a dataset’s columns in a different order than they appear in the dataset. To do that in VisiData, use the following keystrokes:

- `Shift-H`	Moves column one position to the left
- `Shift-L`	Moves column one position to the right

Similarly, you can use Shift-J to move a row down one position, and Shift-K to move a row up one position.
