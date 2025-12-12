# Working with Logs

Read system logs

On nearly all modern Linux distributions, you can access all system logs through journalctl: 

`# journalctl`

As you’ll quickly see, running journalctl without any arguments will drown you in a torrent of data. You’ll need to find some way to filter for the information you’re after. Allow me to introduce you to grep:

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