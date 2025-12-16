# Working with Text Files

## `wc` (word cound)

> count lines, words, and characters

Run without options, it displays all three counts:

```bash
$ wc /etc/passwd
 32  77 2003 /etc/passwd
```

Count the number of byte present too.

In the context of scripting, it is more common to supply a `-l, -w, or -c` option to make wc’s output consist of a single number.

## `tr` (translate)

perform simple character replacements in text or binary data. Useful in cleaning up the results of other commands.

Remove unwanted carriage returns from a file that was created on Windows.

`tr "[A-Z]"  "[a-z]"` => convert all uppercase letters in an input stream to their lowercase equivalents.

## comm

compare the contents of two sorted files.

Determine which lines are common to both files or determine which lines are missing from either file. More formally, `comm` allows you to compute the [union](https://en.wikipedia.org/wiki/Union_(set_theory)) (hội), [intersection](https://en.wikipedia.org/wiki/Intersection_(set_theory)) (giao) and [relative complement](https://en.wikipedia.org/wiki/Complement_(set_theory)) of the lines found in two files.

Btw, you should learn [set theory](https://en.wikipedia.org/wiki/Set_theory) (lý thuyết tập hợp).

## uniq

> print unique lines

Just like `comm`, this command also requires that its input be sorted first (usually by being run through `sort`).

- `uniq` is similar in spirit to `sort -u`, but it has some useful options that sort does not emulate:
  - `-c` to count the number of instances of each line
  - `-d` to show only dupli-cated lines
  - `-u` to show only nonduplicated lines

For example, the command below shows that 20 users have `/bin/bash` as their login shell and that 12 have /bin/false. (The latter are either pseudo-users or users whose accounts have been disabled.)

```bash
$ cut -d: -f7 /etc/passwd | sort | uniq -c
   20 /bin/bash
   12 /bin/false
```

## grep

> search text in files

`grep` searches its input text and prints the lines that match a given pattern. Its name is based on the **g**/regular-expression/**p** command from the old **ed** editor that came with the earliest versions of UNIX (and still does).

- Options:
  * `-c` to print a count of matching lines
  * `-i` to ignore case when matching
  * `-v` to print nonmatching (rather than matching) lines
  * `-l` (lowercase L) makes grep print only the names of matching files rather than printing each line that matches.

```bash
$ sudo grep -l mdadm /var/log/* 
/var/log/auth.log 
/var/log/syslog.0
```

Above command shows that log entries from mdadm have appeared in two different log files.

Search for pieces of matching text in text files such as a csv file that store historical purchase information for products in a store giống dạng excel.

`grep Sneaker sales.csv` Only show lines in file `sales.csv` that contain the string "Sneaker". Work with millions of lines of text.

`grep` can be used with _regular expression_. Ex: extract all the different model number from a csv of sale record.

By default, grep will print the entire line of text where a match is found.\
`-o` flag extract only the matched part of the text.

find and locate are often used in combination with grep to define some serious queries.

**Other useful features:**\

- recursive searches `-R` that look through all files & sub-directory, showing files and line numbers.
- disabling case-sensitivity of the matching.
- inverting matching logic by showing only lines that don't match instead.

## cut

> separate lines into fields

## sed - search & replace

search & replace operation on text files or streams.

Replace all instances of "cat" with "dog".

csv data that contains numbers that are erroneously padded with double quote. In order to correctly process this data, you need to get rid of the double quotes. This can be accomplished with the following `sed` command:  `cat data.csv | sed 's/"//g'`.

The true power of `sed` comes from the fact that it enables you to perform (really) complex changes that can be described by regular expressions. Ex: swap text.

**Back references** allow you to match a variable part of text and then move it around or changes it later depending on how you want to re-write it.

## `awk`

awk giống `sed` can be used to perform text replacements. But `sed` is easier for regex-based replacements. Whereas `awk` can perform arbitrary computations that `sed` can't.

AWK is a _domain-specific language_ designed for text processing and typically used as a data extraction and reporting tool. Like sed and grep, it is a filter, and it is a standard feature of most Unix-like operating systems.

extract the first column.

## `sort`

> sort lines

`sort`'s output can be piped directly into a number of other useful commands.

`sort [OPTION]... [FILE]...`

- Options:
  * `-t`: Set field separator (columns by default are separated by the whitespace character)
  * `-k`: specify the columns that form the sort key. Column index start at one.
  * `-n`: do numerical sort. By default, `sort` uses lexicographical sort (dictionary sort)
  * `-r`: sort in the **reverse order**.
  * `-R`: sort the list **Randomly**. You'll get a different order in each time.
  * `-u`: output unique records only; eliminate duplicate lines chỉ giữ lại 1 dòng

- In `alphabetical sorting` (default), the command checks the first letter of each line and moves the lines upward or downward to arrange each line in alphabetical order.
  * Note: Lines starting with a lowercase letter appear before lines beginning with an uppercase letter.
- In `numerical sorting`, the command checks numbers on each line and arranges the lines in ascending order (default).

```bash
$ sort test.txt
$ sort -r test.txt

$ sort -n numeric.txt
$ sort -nr numeric.txt

$ sort -k 2 file2.txt # sort column 2nd
```

There is a very small difference in sort and `grep` command.  
The sort command arranges data alphabetically or numerically in ascending or descending order. The grep command displays or hides only the required information you want.

---

Both commands below use the `-t:` and `-k3,3` options to sort the `/etc/group` file by its third colon-separated field, the group ID. The first sorts numerically and the second alphabetically.

```bash
$ sort -t: -k3,3 -n /etc/group1
root:x:0:
bin:x:1:daemon
daemon:x:2:
…
$ sort -t: -k3,3 /etc/group 
root:x:0:
bin:x:1:daemon
users:x:100:
…
```

`sort` accepts the key specification `-k3` (rather than `-k3,3`), but it probably doesn’t do what you expect. Without the terminating field number, the sort key continues to the end of the line.

### tsort

`tsort` stands for [topological sort](https://en.wikipedia.org/wiki/Topological_sorting).