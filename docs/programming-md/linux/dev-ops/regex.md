# Regular Expressions

## Basics & Terminologies

- `{}`, `+`, `*` and `?` are called **quantifiers**.  
- `{}` is also called **quantity specifier.**
- flag = modifier

A **metacharacter** in a regular expression is a character that has a special meaning to the regex engine, rather than representing its literal self. Example: `.`, `\d`, `\w`, etc

Regular expressions are neither procedural nor functional languages. Rather, they are a logic-based or declarative language, a class of languages that also includes Prolog and Makefiles. And BNFs. One might also call them rule-based languages. I prefer to call them declarative languages myself.

**alphanumeric**: containing both letters and numbers

Regex patterns are used for four main tasks:

1. to **find** text within a larger body of text
2. to **validate** that a string conforms to a desired format (email, phone number)
3. To **replace** text (or **insert** text at matched positions, which is the same process)
4. And to **split** strings.

Who does this work of finding, replacing, splitting? A **regex engine**. For instance, you can find regex engines in text editors such as Notepad++ and EditPad Pro. You also find regex engines ready to roar in most programming languages—such as C#, Python, Perl, PHP, Java, JavaScript and Ruby.

- `camelCase` is the practice of writing phrases without spaces or punctuation and with capitalized words; first word lower, after that thì uppercase.
- `snake_case`
- `kebab-case`
- `flatcase`
- `PascalCase` is used for Class names
- `SCREAMING_SNAKE_CASE` used for constants

Matching is **case-sensitive** by default.

Many special constructs, such as `+` and `|`, affect the matching of the “thing” to their left or right. In general, a “thing” is a single character, a subpattern enclosed in parentheses, or a character class enclosed in square brackets.

### Escaped characters

- forward-slash `/`
- back-slash `\`

- To enclose the regex pattern, thuận theo chiều gió. Gió thổi left-> right thì `|` nghiên về bên phải thành `/` (forward-slash). Example: `/pattern/gm`
- `/` is also used for finding in `less` & `vim`. Vì nó giống ở trên, thuận theo chiều gió.

- backslash `\` is used to escape characters in regex: `\.`
- `\` is also used for latex commands (`\documentclass{article}`) & escape characters in latex `\&`.
Backslash đi ngược chiều "gió thổi" nên dùng để "escape" characters.
- XML closing tags dùng forwardslash `</h1>`

There are special characters that we use when writing regex. `{ } [ ] / \ + \* . $^ | ?` Before we can select these characters themselves, we need to use an escape character `\` (a back slash). For example, to select the dot . and asterisk \* characters in the text, let's add an escape character \ before it.

Examples:

- `/\./g`: => matches a single “.” character, not the wildcard notation.
- `\\` => dùng để escape a bach slash

## The dot `.` Any Character

The **wildcard character** `.` will match one single character of any kind, including special characters and white space characters (except line breaks). The wildcard is also called dot and period. For example, if you wanted to match ==hug, huh, hut, and hum==, you can use the regex `/hu./` to match all four words.

The escaped character `\.` matches a single literal dot character.

## Flags

Flags change the output of the expression. That's why flags are also called **modifiers**. Flags determine whether the typed expression treats text as separate lines, is case sensitivity, or finds all matches.

You can match both cases using the `i` flag. An example of using this flag is `/ignorecase/i`. This expression can match the three strings: ignorecase, igNoreCase, and IgnoreCase.

The **global flag** `//g` causes the expression to select all matches. If not specified it will only select the _first match_ (return after first match).

Regex by default will sees all text as one line. Kể cả enter newline cũng được coi là một character bình thường.
But we can use the **multi-line flag** `//m` to handle each line separately. In this way, the expressions we write to identify patterns at the start/end of lines (`^` and `$`) work separately for each line.

Many regex systems (including Perl’s) support an `x` option that ignores literal whitespace in the pattern and enables comments, allowing the pattern to be spaced out and split over multiple lines.

## Quantifiers: `+ * ?`

These quantifiers can be used with any character or special metacharacters, for example `a+` (one or more a's), `[abc]+` (one or more of any a, b, or c character) and `.*` (zero or more of any character).

### `+` One or More

`+` allows **one** or more matches of the preceding element => Cộng **một**.

Sometimes, you need to match a single character (or group of characters) that appears **ONE or more** times in a row. This means it occurs at least once, and may be repeated => use the `+` symbol.

Remember, the character or pattern has to be present consecutively. That is, the character has to repeat one after the other.

### `*` Zero or More

`*` allows zero, one, or many matches of the preceding element.

Distinguish: 

- Regex's wildcard is `.` which match any one character.
- Globbing's wildcard is `*`which match any sequence of zero or more characters.
- In regex, the star `*` is a quantifier and it needs something to modify; otherwise, it won’t do what you expect. For example `.*`.

Example:

- `.*` => match an element of unlimited length.
- `/.*buy.*/` => match & filter out all lines/phrases that contain "buy"

### `?` Zero or One

- The `?` symbol có 2 chức năng: optional matching & lazy matching  
- `?` symbol is also used in Lookaround.

Optional symbol `?` allows zero **or one** match of the preceding element.

Sometimes the patterns you want to search for may have parts of it that may or may not exist. However, it may be important to check for them nonetheless. You can specify the possible existence of an element with a question mark `?`. It checks for **zero or one** of the preceding element. You can think of this symbol as saying the previous element is optional.

To accommodate either a five-digit zip code or an extended zip+4: `^\d{5}(-\d{4})?$`  
The parentheses group the dash and extra digits together so that they are considered one optional unit. For example, the regex won’t match a five-digit zip code followed by a dash. If the dash is present, the four-digit extension must be present as well or there is no match.

## Greediness, laziness & catastrophic backtracking

Regular expressions match from left to right. Each component of the pattern matches the longest possible string before yielding to the next component, a characteristic known as **greediness**.

Even though regex notation makes greedy operators the default, they probably shouldn’t be. You should use lazy operators. These versions match as few characters of the input as they can. If that fails, they match more. In many situations, these operators are more efficient and closer to what you want than the greedy versions.

You can apply the regex `/t[a-z]*i/` to the string "titanic". This regex is basically a pattern that starts with t, ends with i, and has some letters in between. Regular expressions are **by default greedy**, so the match would return "titani". It finds the largest sub-string possible to fit the pattern.\
However, you can add a `?` after the `*` or `+` symbols to change them into **lazy matching**.\
The string "titanic" matched against the adjusted regex of `/t[a-z]*?i/` returns "ti".

## Upper-Lower Number of matches `{}`

Recall that you use `+` to look for one or more characters and the asterisk `*` to look for zero or more characters. These are convenient but sometimes you want to match **a certain range** of patterns.  
You can specify the lower and upper number of patterns with `{} `**quantity specifiers** (quantifier).

- `{n}` Matches exactly n instances of the preceding element 
- `{min,}` Matches at least min instances (note the comma)
- `{min,max}` Matches any number of instances from min to max

Example:

- Match only the string "hah" with the letter "a" appearing **at least** 3 times: `/ha{3,}h/`.
- Match only the letter `a` appearing **between** 3 and 5 times in the string "ah": `/a{3,5}h/`.
- Match the word "haaah" with the letter "a" repeat exactly 3 times (no more no less): `/ha{3}h/`.

- This quantifier can be used with any character, or special metacharacters, for example:
  * `w{3}` (three w's)
  * `[wxy]{5}` (five characters, each of which can be a w, x, or y)
  * `.{2,6}` (between two and six of any character)

## Character Classes/Sets `[]`, Ranges & Negated Character Sets

If one of the characters in a word can be in a set of diffrent characters, we put then in **character set/class** notated with square brackets `[]`.

Using the hyphen `-` to match a range of characters is not limited to letters. It also works to match a range of numbers. For example, `/[0-5]/` matches any number between 0 and 5, **including themself**.

So far, you have created a set of characters that you want to match, but you could also create a set of characters that you **do NOT** want to match. These types of character sets are called **negated character sets**. To create a negated character set, you place a caret character `^` after the opening bracket and before the characters you do not want to match `[^chars]`.

NOTE: The `^` symbol is also used to match the beginning of a line together with the `$`.

- Special characters that need to be escaped inside character class:
  - `[` and `]`: start & end of the character class.
  - `\`: for escaping characters `[\\]`.
  - `-` and `^`: character range & class negation

Example:

- `/[^aeiou]/gi` matches all characters that are not a vowel. Note that characters like ., !, [, @, / and white space are matched - the negated vowel character set only excludes the vowel characters.
- `[a-z]`, `[a-zA-Z]`, `[0-9]`, `[a-f]`, `/a-z0-9/ig`
- `/http[^s].*/` => match "http" not "https"
- `/b[aiu]g/` => match "bag", "big", "bug"
- `/[fc]at/g` => match "fat" and "cat"
- `/[Gg]r[ae]y/` => match 4 different spellings and capitalization of the word "gray/grey" at once.

### Character Set `[]` Shorthands

Using character sets, you were able to search for all letters of the alphabet with `[a-z]`. This kind of character class is common enough that there is a shorthand for it, although it still includes a few characters to learn.

Mấy cái này dùng syntax `\` giống escaped characters.

---

**\w and \W (alphanumeric & underscore)**

The first shorthand is `\w`. This shortcut is equal to `[A-Za-z0-9\_]`. Match alphanumeric & underscore.

The opposite of the \w is `\W`. Note, the opposite pattern uses a capital letter. This shortcut is the same as `[^A-Za-z0-9\_]`. Including: `[empty space].:!?`

---

**\d - Any digit**

The shortcut to look for digit characters is `\d`, with a lowercase d. This is equal to the character class `[0-9]`, which looks for a single character of any number between zero and nine.

The shortcut to look for non-digit characters is `\D`. This is equal to the character class `[^0-9]`, which looks for a single character that is not a number between zero and nine.

---

**Match white spaces**

The most common forms of whitespace you will use with regular expressions are the **space** `␣`, the **tab** `\t`, the **new line** `\n` and the carriage return `\r` (useful in Windows environments), and these special characters match each of their respective whitespaces.

You can search for whitespace characters using `\s`, which is a lowercase s. This pattern not only matches whitespaces characters, but also carriage return (enter key), tab, form feed, and new line characters or line breaks. You can think of it as similar to the character class `[\r\t\f\n\v]` (that is, a space, a form feed, a tab, a newline, or a return).

Search for non-whitespace using `\S`, equal to the character class `[^ \r\t\f\n\v]`.

---

- `\W` là "except word character"
- `\D` là "except digit"
- `\S` là "except white space"

You can specify the lower and upper number of patterns with quantity specifiers (quantifier). Quantity specifiers are used with curly brackets ({ and }). You put two numbers between the curly brackets - for the lower and upper number of patterns. Both {}, +, \*, ? đều thuộc category 'quantifiers'. For example, to match only the letter a appearing between 3 and 5 times in the string ah, your regex would be /a{3,5}h/.

Sometimes you only want to specify the lower number of patterns with no upper limit. To only specify the lower number of patterns, keep the first number followed by a comma. For example, to match only the string hah with the letter a appearing at least 3 times, your regex would be /ha{3,}h/.

Sometimes you only want a specific number of matches. To specify a certain number of patterns, just have that one number between the curly brackets. For example, to match only the word hah with the letter a 3 times, your regex would be /ha{3}h/.

**Other**

Additionally, there is a special metacharacter \b which matches the boundary between a word and a non-word character. It's most useful in capturing entire words (for example by using the pattern \w+\b).

## Capture Groups `()`

We can group an expression and use these groups to reference or enforce some rules. To group an expression, we enclose them in parentheses `()`.

- `(expr)`: Limits scope, groups elements, allows matches to be captured
- `(){}` capture groups with min-max
- `([AEae]l[- ])?` two character sets inside capture group and an optional `?` quantifier

### Capture Groups with The Alternation Symbol `|`

The pipe character `|` (also called the "Or" operator) allows to specify that an expression can be in different expressions. Thus, all possible statements are written separated by the pipe sign `|`. This differs from charset `[abc]`, charsets operate at the character level while (`|`) alternatives are at the expression level. 

For example, the following expression would select both "cat" and "rat": `/(c|r)at/g`.\
If you want to find either Penguin or Pumpkin in a string, you can use the following regex: `/P(engu|umpk)in/g`

For the `|` character, however, thingness extends indefinitely to both left and right. If you want to limit the scope of the vertical bar, enclose the bar and both things in their own set of parentheses. For example,

`I am the (walrus|egg man)\.` => matches either “I am the walrus.” or “I am the egg man.”. This example also dem-onstrates escaping of special characters (here, the dot).

### Reuse patterns using Capture Groups
anki
When a match succeeds, every set of parentheses becomes a “capture group” that records the actual text that it matched.

Since parentheses can nest, how do you know which match is which? Easy—the matches arrive in the same order as the opening parentheses. There are as many captures as there are opening parentheses, regardless of the role (or lack of role) that each parenthesized group played in the actual matching. When a parenthe-sized group is not used (e.g., `Mu(')?ammar` when matched against “Muammar”), its corresponding capture is empty.

If a group is matched more than once, only the contents of the last match are returned. For example, with the pattern `(I am the (walrus|egg man)\. ?){1,2}` matching the text "I am the egg man. I am the walrus." There are two results, one for each set of parentheses:

1. I am the walrus. 
2. walrus

Note that both capture groups actually matched twice. However, only the last text to match each set of parentheses is actually captured.

This is also know as referencing a group. Say you want to match a word that occurs multiple times.

The substring matched by the group is saved to a temporary "variable", which can be accessed within the same regex using a backslash and the number of the capture group (e.g. \1). Capture groups are automatically numbered by the position of their opening parentheses (left to right), starting at 1.

All the quantifiers including the star `*`, `+`, repetition {m,n} and the question mark ? can all be used within the capture group patterns. This is the only way to apply quantifiers on sequences of characters instead of the individual characters themselves.

The example below matches a word that occurs thrice separated by spaces:

**Non-capturing group (?: )**

You can group an expression and ensure that it is not captured by references. For example, below are two groups. However, the first group reference we denote with \1 actually indicates the second group, as the first is a non-capturing group.

`/(?:ha)-ha,(haa)-\1/g`

**Nested groups**

When you are working with complex data, you can easily find yourself having to extract multiple layers of information, which can result in nested groups. Generally, the results of the captured groups are in the order in which they are defined (in order by open parenthesis).

The nested groups are read from left to right in the pattern, with the first capture group being the contents of the first parentheses group, etc.

For the following strings, write an expression that matches and captures both the full date, as well as the year of the date.

### Use Capture Groups to search and replace

You can search and replace text in a string using .replace() on a string. The inputs for .replace() is first the regex pattern you want to search for. The second parameter is the string to replace the match or a function to do something.

You can also access capture groups in the replacement string with dollar signs ($).

## Lookaround {lookaround}

If we want the phrase we're writing to come before or after another phrase, we need to "lookaround". There are four types of lookaround.

Phần lookaround không được select. Nó chỉ dùng như điều kiện.

**Positive & Negative Lookahead: (?=) (?!)**

Placed after the pattern that you want to match.

`/\d+(?=PM)/g` => match digit that have "PM" after them like "3PM"

**Positive & Negative Lookbehind: (?<=) (?<!)**

Thêm dấu `<` vào Lookahead, placed before the pattern that you want to match.

`/(?<=\$)\d+/g` => match $"5" but not "1064"

### Lookahead

Lookaheads are patterns that tell JavaScript to look-ahead in your string to check for patterns further along. There are two kinds of lookaheads: positive lookahead and negative lookahead.

- A **positive lookahead** will look to make sure the element in the search pattern is there, but won't actually match it. A positive lookahead is used as (?=...) where the ... is the required part that is not matched.
- On the other hand, a **negative lookahead** will look to make sure the element in the search pattern is not there. A negative lookahead is used as (?!...) where the ... is the pattern that you do not want to be there. The rest of the pattern is returned if the negative lookahead part is not present.

Lookaheads are a bit confusing but some examples will help.

For example, we want to select the hour value in the text. Therefore, to select only the numerical values that have PM after them, we need to write the positive look-ahead expression (?=) after our expression. Include PM after the = sign inside the parentheses.

For example, we want to select numbers other than the hour value in the text. Therefore, we need to write the negative look-ahead (?!) expression after our expression to select only the numerical values that do not have PM after them. Include PM after the ! sign inside the parentheses.

A more practical use of lookaheads is to check two or more patterns in one string. Here is a (naively) simple password checker that looks for between 3 and 6 characters and at least one number:

Use lookaheads in the pwRegex to match passwords that are greater than 5 characters long, and have two consecutive digits.

### Lookbehind

Positive LookBehind (?<=): matches a group before the main expression without including it in the result. Example: /(?<=[tT]he)./g => matches every first character preceded by the word “the/The”

Negative Lookbehind (?<!): specifies a group that can not match before the main expression (if it matches, the result is discarded)

For example, we want to select the price value in the text. Therefore, to select only the number values that are preceded by $, we need to write the positive lookbehind expression (?<=) before our expression. Add \$ after the = sign inside the parenthesis.

For example, we want to select numbers in the text other than the price value. Therefore, to select only numeric values that are not preceded by $, we need to write the negative lookbehind (?<!) before our expression. Add \$ after the ! inside the parenthesis.

## Match Beginning & Ending of a String

In an earlier challenge, you used the caret character `^` inside a character set `[]` to create a [negated character set](#character-sets--ranges-and-negated-character-sets) in the form `[^thingsThatWillNotBeMatched]`. But **outside** of a character set, the caret is used to search for patterns at the **beginning of strings**.

If the multiline flag (`m`) is enabled, `^` will match the beginning of lines instead of the whole string. Dùng khi có 1 big string là 1 paragraph contains multiple lines. Nếu không có (m) flag thì "^" chỉ match the beginning of the first line.

---

You can search the end of strings using the dollar sign character `$` at the end of the regex.

If the multiline flag (m) is enabled, `$` will match the end of a line instead of the whole string.

`/html$/gm` find the "html" texts only at the end of the line.

**Examples:**

`/^[0-9]/gm` => match numbers at the beginning of a line

`/^http[^s].*/` => match "<http://httpstatus.io/>"; "http" at the beginning

In `^\d{5}$`, The `^` and $ match the beginning and end of the search text but do not actually correspond to characters in the text; they are **zero-width assertions.** These char-acters ensure that only texts consisting of exactly five digits match the regular expression—the regex will not match five digits within a larger string. The \d es-cape matches a digit, and the quantifier {5} says that there must be exactly five digit matches.

## My regex patterns

Validate email: ``

## Linux Globbing

`grep` uses regular expressions (regex).  
The filename matching and expansion performed by the shell when it interprets command lines such as `wc -l *.pl` is not a form of regex matching. It’s a different system called **shell globbing,** and it uses a different and simpler syntax.

Glob mean "global commands". [/etc/glob](https://en.wikipedia.org/wiki/Glob_(programming)) is a program in UNIX V6 that would expand wildcard patterns. Soon afterward, this became a shell built-in.

Globbing is mainly used to match filenames or searching for content in a file. Globbing uses wildcard characters to create the pattern.

Globbing is used in config files such as a `.gitignore` where you might see `.cache/*`, for example.

Globbing is the expansion of simple pattern-matching characters such as * and ? to form filenames or lists of file-names)

Globbing is an operation that is performed by the shell itself and it happens **independently** of the actual command we're running. The shell will **first** attemp to expand the wildcard pattern to match any files that are present before passing their expanded names to the program we want to run.  
`man 7 glob` to read more.

You should be aware that the value of the arguments that get passed to a given program will actually depend on the content of your file system. Your program may be passed a different number of arguments depending on how many files match the wildcard.

We should note that while globbing might look similar to regular expressions, they’re fundamentally different. While the patterns seem similar, globbing doesn’t use regular expressions.

- We use shell-style globbing characters for pattern matching:
  * A star (`*`) matches zero or more characters.
  * A question mark (`?`) matches any single character. You can use `?` for multiple times for matching multiple characters.
  * A tilde or “twiddle” (`~`) means the home directory of the current user
  * `~user` means the home directory of user.

For example, we might refer to the startup script directories `/etc/rc0.d`, `/etc/rc1.d`, and so on with the shorthand pattern `/etc/rc*.d`.

- `[]` range of characters. For example `[0-9]` or `[abc]`. Ranges can apply to letters as well as digits:
- `[a-z]` = all lowercase characters of the alphabet
- `[A-Z]` = all uppercase characters of the alphabet
- `[a-zA-Z]` = all characters of the alphabet, irrespective of their case
- `[j-p]` = lowercase characters j, k, l, m, n, o or p
- `[a-z3-6]` = lowercase characters or the numbers 3, 4, 5 or 6

`[]` is used to match the character from the range. Some of the mostly used range declarations are mentioned below.

- All uppercase alphabets are defined by the range as, `[:upper:] or [A-Z]` .
- All lowercase alphabets are defined by the range as, \[:lower:] or \[a-z].
- All numeric digits are defined by the range as, \[:digit:] or \[0-9].
- All uppercase and lower alphabets are defined by the range as, \[:alpha:] or \[a-zA-z].
- All uppercase alphabets, lowercase alphabet and digits are defined by the range as, \[:alnum:] or \[a-zA-Z0-9]

By default, **hidden files and folders** don’t show up in the output of ls. To apply globbing to our hidden files and folders, we have to explicitly add a leading `.` (dot).  
`ls *` vs `ls .*`

Examples:

- `ls *.txt`
- `ls test-?.txt` => list all text files named ‘test-‘ followed by a single digit
- `ls ????.txt` => all text files with a name of exactly four characters
- `ls *[0-9]*` => all files with a number in their name

### Single vs. Double Quotes

The shell treats strings enclosed in single and double quotes similarly, except that **double-quoted** strings are subject to globbing (the expansion of filename-match-ing metacharacters such as * and ?) and variable expansion.

Back-ticks, are treated similarly to double quotes, but they have the additional effect of executing the contents of the string as a shell command and replacing the string with the command’s output.

```bash
$ echo "There are `wc -l /etc/passwd` lines in the passwd file." 
There are 28 lines in the passwd file.
```

Single and double quotes are often used in Linux bash commands or scripts, especially when dealing with filenames. Although both quote types prevent globbing and word splitting, it is important to pay attention to the quotes you use. The differences between the quote types make them noninterchangeable in some cases.

`'!$"*<>` is typically the order of precedence for those symbols.

Đôi khi phải dùng single quote to pass unchanged wildcard pattern to programs. Prevent the shell to expand them before passign to programs.

## References

[regexLearn.com](https://regexlearn.com/learn)

[RegexOne.com](https://regexone.com/)

Learn Regex on FreeCodeCamp

[regex101](https://regex101.com/) for testing, playground

[regexLearn](https://regexlearn.com/)

[rexegg.com](https://www.rexegg.com/) not basic, but advace

Jan Goyvaerts's excellent [regular-expression.info](https://www.regular-expressions.info/) page, more tutorial style
