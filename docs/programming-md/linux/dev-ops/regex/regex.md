# Regular Expressions Notes

TODO

- đọc lại again [anchor](https://www.regular-expressions.info/anchors.html)
- đang đọc tới [repetition](https://www.regular-expressions.info/repeat.html)

## Basics & Terminologies

**Literals** là khi `a` match `a`; `<` match `<` gọi là literals.

- `{}`, `+`, `*` and `?` are called **quantifiers**.  
- `{}` is also called **quantity specifier.**
- flag = modifier

A **metacharacter** in a regular expression is a character that has a special meaning to the regex engine, rather than representing its literal self. In regex, there are 12 special characters. Example: `.`, `\d`, `\w`, etc

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

A regex engine always returns the leftmost match, even if a “better” match could be found later.

## Escaped characters

- forward-slash `/`
- back-slash `\`

- To enclose the regex pattern, thuận theo chiều gió. Gió thổi left-> right thì `|` nghiên về bên phải thành `/` (forward-slash). Example: `/pattern/gm`
- `/` is also used for finding in `less` & `vim`. Vì nó giống ở trên, thuận theo chiều gió.

- backslash `\` is used to **escape** characters in regex: `\.`
- `\` is also used for latex commands (`\documentclass{article}`) & escape characters in latex `\&`.
Backslash đi ngược chiều "gió thổi" nên dùng để "escape" characters.
- XML closing tags dùng forwardslash `</h1>`

There are special characters that we use when writing regex. `{ } [ ] / \ + \* . $^ | ?` Before we can select these characters themselves, we need to use an escape character `\` (a back slash). For example, to select the dot . and asterisk \* characters in the text, let's add an escape character \ before it.

Examples:

- `/\./g`: => matches a single “.” character, not the wildcard notation.
- `\\` => dùng để escape a bach slash

---

In your source code, you have to keep in mind which characters get special treatment inside strings by your programming language. That is because those characters are processed by the compiler, before the regex library sees the string. So the regex `1\+1=2` must be written as `"1\\+1=2"` in C++ code. The C++ compiler turns the escaped backslash in the source code into a single backslash in the string that is passed on to the regex library. To match `c:\temp`, you need to use the regex `c:\\temp`. As a string in C++ source code, this regex becomes `"c:\\\\temp"`. Four backslashes to match a single one indeed.

## Unicode

### Unicode Categories

The Unicode standard recommends that regular expression engines support the `\p{Property_Set=Property_Value}` syntax to match any character that has the specified value for the specified property. For example, `\p{Numeric_Value=1}+` should match all of `1¹١۱१১৴੧૧୧௧౧೧൧๑໑༡₁⅟Ⅰⅰ①⑴⒈❶➀➊〡㊀１`.

The syntax `\p{Property}` should be used to match any character that has the specified binary property. `\p{Hex_Digit}+` should match `0123456789ABCDEFabcdef０１２３４５６７８９ＡＢＣＤＥＦａｂｃｄｅｆ`.  
Unicode suggests this shorter notation as an alternative for categories and scripts. So you could use `\p{Nd}` instead of `\p{gc=Nd}` and `\p{Common}` instead of `\p{Script=Common}`.

Different regex flavors can support different Unicode properties. Every flavor that supports Unicode properties supports `\p{N}` and `\p{Nd}` to match all characters in a specific Unicode category, specifying just the one-letter or two-letter abbreviation for the category. Far fewer flavors support the more explicit syntax `\p{gc=Nd}`.

Many flavors also support Unicode scripts specifying just the name of the script. `\p{Cyrillic}` matches all characters in the Cyrillic script. Most, but not all, of those also support `\p{Script=Cyrillic}`.

Some flavors support Unicode blocks by specifying the name of the block with the prefix `In`. `\p{InCyrillic}` is equivalent to `[\u{0400}-\u{04FF}]`, matching this entire block. Again, most, but not all, of those flavors also support `\p{Block=Cyrillic}`. The prefix is needed to differentiate between the Unicode script of the same name.

All flavors that support some Unicode properties also support the syntax with a capital `P` to negate the property, as recommended by Unicode. `\P{Nd}` matches any code point that is not in the `Decimal_Digit` category. That includes unassigned code points. Perl, Ruby, PCRE, PCRE2, and flavors based on the latter two such as PHP, Delphi, and R, support an alternative syntax using a caret for negation. `\p{^Nd}` is another way of writing `\P{Nd}` in those flavors. Be careful not to negate the property twice. `\P{^Nd}` with double negation is the same as `\p{Nd}` without any negation.

---

Each Unicode character belongs to a certain category. Unicode categories, or “general categories” as they’re called by the Unicode standard, are the most fundamental Unicode property. Every regex flavor that supports Unicode properties at all supports Unicode categories. That includes .NET, Java, ICU, JavaScript with /u, Ruby, JGsoft, Perl, PCRE, PCRE2.

All these flavors support the `\p{Property}` syntax with the property being a single letter or two letter representing the category. You can match a single character belonging to the “letter” category with `\p{L}`. You can match a single character not belonging to that category with `\P{L}`. `\p{Ll}` matches a lowercase letter while `\P{Ll}` matches any character that is not a lowercase letter.

Again, “character” really means “Unicode code point”. `\p{L}` matches a single code point in the category “letter”. If your input string is à encoded as `U+0061 U+0300` then it matches a without the accent. If the input is à encoded as U+00E0 then it matches à with the accent. The reason is that both the code points U+0061 (a) and U+00E0 (à) are in the category “letter”, while U+0300 is in the category “mark”.

ICU, Perl, Ruby, JavaScript, and the JGsoft applications allow you to spell out the full category names, such as \p{Letter} or `\p{Lowercase_Letter}`.

PCRE and .NET are case sensitive for the category letters. `\p{Zs}` will match any kind of space character, while `\p{zs}` will throw an error. It’s best to stick with the capitalization required by the case sensitive flavors. It is how the category letters are defined in Unicode. It will make your regular expressions work with all Unicode regex engines.

## Non-Printable Characters

- `\t` match a tab character
- `\r` match one carriage return; `\n` for line feed
  - Windows text files use `\r\n` to terminate lines, while UNIX text files use `\n`. Some flavors use `\R` to match a single line break and treat `\r\n` as an indivisible pair.
- `\s`match: space, tab `\t`, newline `\n`, carriage return `\r`, or form feed `\f`
  - Typing out a literal space bar `␣` will matches only a standard space character, ignoring tabs and newlines.
- `\v` matches any vertical whitespace character. That includes the vertical tab, form feed, and all line break characters.

---

 Remove empty lines

Remove ALL empty lines (Including those with spaces/tabs)

- Find: `^\s*\r?\n` => Replace: (Leave completely empty)
  - The `\s*` will group multiple consecutive empty lines into one single match.
  - `\r?\n` matches the line break (works on both Mac/Linux and Windows). If your flavor supports the shorthand `\v` to match any line break character, then use it.
- or `^\s*$\n?`
  - `$` is the end of line

Remove ONLY completely blank lines (Zero characters)

- Find: `^\r?\n`
- Replace: (Leave completely empty)

## The dot `.` Any Character

The **wildcard character** `.` will match one single character of any kind, including special characters and white space characters (except only line breaks). The wildcard is also called dot and period. For example, if you wanted to match ==hug, huh, hut, and hum==, you can use the regex `/hu./` to match all four words.

The dot matches a single character, without caring what that character is. The only exception are line break characters. In all regex flavors discussed in this tutorial, the dot does **NOT** match line breaks by default

This exception exists mostly because of historic reasons. The first tools that used regular expressions were line-based. They would read a file line by line, and apply the regular expression separately to each line. The effect is that with these tools, the string could never contain line breaks, so the dot could never match them.  
Modern tools and languages can apply regular expressions to very large strings or even entire files. Except for VBScript, all regex flavors discussed here have an option to make the dot match all characters, including line breaks. Older implementations of JavaScript don’t have the option either. It was formally added in the ECMAScript 2018 specification.

In JavaScript (for compatibility with older browsers) and VBScript you can use a character class such as `[\s\S]` to match any character. This character matches a character that is either a whitespace character (including line break characters), or a character that is not a whitespace character. Since all characters are either whitespace or non-whitespace, this character class matches any character. Do not use alternation like (`\s|\S`) which is slow. And certainly don’t use (.|\s) which can lead to catastrophic backtracking as spaces and tabs can be matched by both . and \s.

The escaped character `\.` matches a single literal dot character.

---

Use the dot **sparingly**. Often, a character class or negated character class is faster and more precise.

The dot is a very powerful regex metacharacter. It allows you to be lazy. Put in a dot, and everything matches just fine when you test the regex on valid data. The problem is that the regex also matches in cases where it should not match. If you are new to regular expressions, some of these cases may not be so obvious at first.

Let’s illustrate this with a simple example. Say we want to match a date in mm/dd/yy format, but we want to leave the user the choice of date separators. The quick solution is \d\d.\d\d.\d\d. Seems fine at first. It matches a date like 02/12/03 just fine. Trouble is: 02512703 is also considered a valid date by this regular expression. In this match, the first dot matched 5, and the second matched 7. Obviously not what we intended.  
`\d\d[- /.]\d\d[- /.]\d\d` is a better solution. This regex allows a dash, space, dot and forward slash as date separators. Remember that the dot is not a metacharacter inside a character class, so we do not need to escape it with a backslash.

This regex is still far from perfect. It matches 99/99/99 as a valid date. `[01]\d[- /.][0-3]\d[- /.]\d\d` is a step ahead, though it still matches 19/39/99. How perfect you want your regex to be depends on what you want to do with it. If you are validating user input, it has to be perfect. If you are parsing data files from a known source that generates its files in the same way every time, our last attempt is probably more than sufficient to parse the data without errors. You can find a better regex to match dates in the example section.

## Flags

Flags change the output of the expression. That's why flags are also called **modifiers**. Flags determine whether the typed expression treats text as separate lines, is case sensitivity, or finds all matches.

regex engines are case sensitive by default. But you can tell it to match both cases using the `i` flag. An example of using this flag is `/ignorecase/i`. This expression can match the three strings: ignorecase, igNoreCase, and IgnoreCase.

The **global flag** `//g` causes the expression to select all matches. If not specified it will only select the _first match_ (return after first match).

Regex by default will sees all text as one line. Kể cả enter newline cũng được coi là một character bình thường.
But we can use the **multi-line flag** `//m` to handle each line separately. In this way, the expressions we write to identify patterns at the start/end of lines (`^` and `$`) work separately for each line.

Many regex systems (including Perl’s) support an `x` option that ignores literal whitespace in the pattern and enables comments, allowing the pattern to be spaced out and split over multiple lines.

## Repetition with `*` and `+`

The `repetition quantifiers` or operators (`* + ?`) can be used with any character or special metacharacters, for example `a+` (one or more a's), `[abc]+` (one or more of any a, b, or c character) and `.*` (zero or more of any character).

`+` allows **one or more** matches of the preceding element => Cộng **một**.

Sometimes, you need to match a single character (or group of characters) that appears **ONE or more** times in a row. This means it occurs at least once, and may be repeated => use the `+` symbol.

Remember, the character or pattern has to be present consecutively. That is, the character has to repeat one after the other.

---

`*` allows zero, one, or many matches of the preceding element (**zero or more**).

Distinguish:

- Regex's wildcard is `.` which match any one character.
- Globbing's wildcard is `*`which match any sequence of zero or more characters.
- In regex, the star `*` is a quantifier and it needs something to modify; otherwise, it won’t do what you expect. For example `.*`.

Example:

- `.*` => match an element of unlimited length.
- `/.*buy.*/` => match & filter out all lines/phrases that contain "buy"

The asterisk or star tells the engine to attempt to match the preceding token zero or more times. The plus tells the engine to attempt to match the preceding token once or more. `<[A-Za-z][A-Za-z0-9]*>` matches an HTML tag without any attributes. The angle brackets are literals. The first character class matches a letter. The second character class matches a letter or digit. The star repeats the second character class. Because we used the star, it’s OK if the second character class matches nothing. So our regex will match a tag like `<B>`. When matching `<HTML>`, the first character class will match H. The star will cause the second character class to be repeated three times, matching T, M and L with each step.

### Limiting Repetition `{}`

Recall that you use `+` to look for one or more characters and the asterisk `*` to look for zero or more characters. These are convenient but sometimes you want to match **a certain range** of patterns.  
You can specify the lower and upper number of patterns with `{}`**quantity specifiers** (quantifier).

- `{n}` Matches exactly n instances of the preceding element
- `{min,}` Matches at least min instances (note the comma)
- `{min,max}` Matches any number of instances from min to max

Example:

- Match only the string "hah" with the letter "a" appearing **at least** 3 times: `/ha{3,}h/`.
- Match only the letter `a` appearing **between** 3 and 5 times in the string "ah": `/a{3,5}h/`.
- Match the word "haaah" with the letter "a" repeat exactly 3 times (no more no less): `/ha{3}h/`.

- This quantifier can be used with any character, or special metacharacters, for example:
  - `w{3}` (three w's)
  - `[wxy]{5}` (five characters, each of which can be a w, x, or y)
  - `.{2,6}` (between two and six of any character)

You could use `\b[1-9][0-9]{3}\b` to match a number between 1000 and 9999. `\b[1-9][0-9]{2,4}\b` matches a number between 100 and 99999. Notice the use of the word boundaries.

### Greediness, laziness & catastrophic backtracking

Regular expressions match from left to right. Each component of the pattern matches the longest possible string before yielding to the next component, a characteristic known as **greediness**.

Even though regex notation makes greedy operators the default, they probably shouldn’t be. You should use lazy operators. These versions match as few characters of the input as they can. If that fails, they match more. In many situations, these operators are more efficient and closer to what you want than the greedy versions.

You can apply the regex `/t[a-z]*i/` to the string "titanic". This regex is basically a pattern that starts with t, ends with i, and has some letters in between. Regular expressions are **by default greedy**, so the match would return "titani". It finds the largest sub-string possible to fit the pattern.\
However, you can add a `?` after the `*` or `+` symbols to change them into **lazy matching**.\
The string "titanic" matched against the adjusted regex of `/t[a-z]*?i/` returns "ti".

Most people new to regular expressions will attempt to use `<.+>`. They will be surprised when they test it on a string like `This is a <EM>first</EM> test`. You might expect the regex to match `<EM>` and when continuing after that match, `</EM>`.

But it does not. The regex will match `<EM>first</EM>`. Obviously not what we wanted. The reason is that the plus is _greedy_. That is, the plus causes the regex engine to repeat the preceding token as often as possible. Only if that causes the entire regex to fail, will the regex engine _backtrack_. That is, it will go back to the plus, make it give up the last iteration, and proceed with the remainder of the regex

Like the plus, the star and the repetition using curly braces are greedy.

---

The quick fix to this problem is to make the plus lazy instead of greedy. Lazy quantifiers are sometimes also called “ungreedy” or “reluctant”. You can do that by putting a question mark after the plus in the regex. You can do the same with the star, the curly braces and the question mark itself. So our example becomes `<.+?>`.

In this case, there is a better option than making the plus lazy. We can use a greedy plus and a negated character class: `<[^>]+>`. The reason why this is better is because of the backtracking. When using the lazy plus, the engine has to backtrack for each character in the HTML tag that it is trying to match. When using the negated character class, no backtracking occurs at all when the string contains valid HTML code. Backtracking slows down the regex engine.

## Optional Items `?`

- The `?` symbol có 2 chức năng: optional matching & lazy matching  
- `?` symbol is also used in Lookaround.

Optional symbol `?` allows zero **or once** match of the preceding element.

You can make several tokens optional by grouping them together using parentheses, and placing the question mark after the closing parenthesis. E.g.: `Nov(ember)?` matches `Nov` and `November`.

You can write a regular expression that matches many alternatives by including more than one question mark. `Feb(ruary)? 23(rd)?` matches `February 23rd`, `February 23`, `Feb 23rd` and `Feb 23`.

Sometimes the patterns you want to search for may have parts of it that may or may not exist. However, it may be important to check for them nonetheless. You can specify the possible existence of an element with a question mark `?`. It checks for **zero or one** of the preceding element. You can think of this symbol as saying the previous element is optional.

To accommodate either a five-digit zip code or an extended zip+4: `^\d{5}(-\d{4})?$`  
The parentheses group the dash and extra digits together so that they are considered one optional unit. For example, the regex won’t match a five-digit zip code followed by a dash. If the dash is present, the four-digit extension must be present as well or there is no match.

---

The question mark is the first metacharacter introduced by this tutorial that is greedy. The question mark gives the regex engine two choices: try to match the part the question mark applies to, or do not try to match it. The engine always tries to match that part. Only if this causes the entire regular expression to fail, will the engine try ignoring the part the question mark applies to.

The effect is that if you apply the regex `Feb 23(rd)?` to the string `Today is Feb 23rd, 2003` then the match is always `Feb 23rd` and not `Feb 23`. You can make the question mark lazy (i.e. turn off the greediness) by putting a second question mark after the first.

## Character Classes/Sets `[]`

A character class/set `[]` matches only one out of several characters. A character class matches only a single character.

Using the hyphen `-` to match a range of characters is not limited to letters. It also works to match a range of numbers. For example, `/[0-5]/` matches any number between 0 and 5, **including themself**.  
A literal `\-` ở trong character set thì phải escape, ở ngoài thì không cần

- `[0-9a-fA-F]` matches a single hexadecimal digit, case insensitively.
- You can combine ranges and single characters. `[0-9a-fxA-FX]` matches a hexadecimal digit or the letter X.
- `[0-9a-fxA-FX]` matches a hexadecimal digit or the letter X. Again, the order of the characters and the ranges does not matter.
- `[a-z]`, `[a-zA-Z]`, `[0-9]`, `[a-f]`, `/a-z0-9/ig`

---

A **Negated Character Set** matches any character that is not in the character class.

To create a negated character set, you place a caret character `^` after the opening bracket and before the characters you do not want to match `[^chars]`.

It is important to remember that a negated character class still must match a character. `q[^u]` does not mean: “a q not followed by a u”. It means: “a q followed by a character that is not a u”. It does not match the q in the string `Iraq`. It does match the q and the space after the q in Iraq is a country. Indeed: the space becomes part of the overall match, because it is the “character that is not a u” that is matched by the negated character class in the above regexp. If you want the regex to match the q, and only the q, in both strings, you need to use negative lookahead: `q(?!u)`. But we will get to that later.

NOTE: The `^` symbol is also used to match the beginning of a line together with the `$`.

- Special characters that need to be escaped inside character class:
  - `[` and `]`: start & end of the character class.
  - `\`: for escaping characters `[\\]`. Example: `[\\x]` matches a backslash or an x.
  - `-` and `^`: character range & class negation
- The usual metacharacters are normal characters inside a character class, and do not need to be escaped by a backslash. To search for a star or plus, use `[+*]`. Your regex will work fine if you escape the regular metacharacters inside a character class, but doing so significantly reduces readability.

- The closing bracket ], the caret ^ and the hyphen - can be included by escaping them with a backslash, or by placing them in a position where they do not take on their special meaning.
- To include an unescaped caret as a literal, place it anywhere except right after the opening bracket. `[x^]` matches an x or a caret.

Example:

- `/[^aeiou]/gi` matches all characters that are not a vowel. Note that characters like ., !, [, @, / and white space are matched - the negated vowel character set only excludes the vowel characters.
- `/http[^s].*/` => match "http" not "https"
- `/b[aiu]g/` => match "bag", "big", "bug"
- `/[fc]at/g` => match "fat" and "cat"
- `/[Gg]r[ae]y/` => match 4 different spellings and capitalization of the word "gray/grey" at once.
- `[^0-9\r\n]` matches any character that is not a digit, carriage return, or line feed.

---

Repeating Character Classes

If you repeat a character class by using the `?`, `*` or `+` operators then you’re repeating the entire character class. You’re not repeating just the character that it matched. The regex `[0-9]+` can match 837 as well as 222.

If you want to repeat the matched character, rather than the class, then you need to use backreferences. `([0-9])\1+` matches `222` but not `837`. When applied to the string 833337, it matches 3333 in the middle of this string. If you do not want that then you need to use word boundaries or lookaround.

---

The regex `gr(a|e)y` uses alternation instead of a character class.

### Shorthand Character Sets

Using character sets, you were able to search for all letters of the alphabet with `[a-z]`. This kind of character class is common enough that there is a shorthand for it, although it still includes a few characters to learn.

Mấy cái này dùng syntax `\` giống escaped characters.

---

The first shorthand is `\w` (alphanumeric & underscore). This shortcut is equal to `[A-Za-z0-9\_]`. Match alphanumeric & underscore (có cả digit `[0-9]` luôn).

The opposite of the \w is `\W`. Note, the opposite pattern uses a capital letter. This shortcut is the same as `[^A-Za-z0-9\_]`. Including: `[empty space].:!?`

---

The shortcut to look for digit characters is `\d` (any digit), with a lowercase d. This is equal to the character class `[0-9]`, which looks for a single character of any number between zero and nine.

The shortcut to look for non-digit characters is `\D`. This is equal to the character class `[^0-9]`, which looks for a single character that is not a number between zero and nine.

Consider whether `[0-9]` or `\d` is more appropriate for your regex as the latter may include digits in many different writing systems.

---

`\s` (white space) matches both horizontal whitespace (spaces and tabs) and vertical whitespace (line breaks).  it includes `[ \t\r\n\f]`. That is: \s matches a space, a tab, a carriage return, a line feed, or a form feed. Most flavors also include the vertical tab

`[\da-fA-F]` matches a hexadecimal digit, and is equivalent to `[0-9a-fA-F]` if your flavor only matches ASCII characters with `\d`.

Many regex flavors support `\h` to match only horizontal whitespace, which includes the tab and all characters in the “space separator” Unicode category. It is the same as `[\t\p{Zs}]`.

and `\v` to match only vertical whitespace, which includes all characters treated as line breaks in the Unicode standard. It is the same as `[\n\cK\f\r\x85\x{2028}\x{2029}]`.

If your flavor supports \h and \v then you should definitely use them instead of \s whenever you want to match only one type of whitespace. Using \h instead of \s to match spaces and tabs makes sure your regex match doesn’t accidentally spill into the next line.

The most common forms of whitespace you will use with regular expressions are the space `␣`, the tab `\t`, the new line `\n` and the carriage return `\r` (useful in Windows environments), and these special characters match each of their respective whitespaces.

You can search for whitespace characters using `\s`, which is a lowercase s. This pattern not only matches whitespaces characters, but also carriage return (enter key), tab, form feed, and new line characters or line breaks.

Search for non-whitespace using `\S`, equal to the character class `[^ \r\t\f\n\v]`.

---

- `\W` là "except word character"
- `\D` là "except digit"
- `\S` là "except white space"

You can specify the lower and upper number of patterns with quantity specifiers (quantifier). Quantity specifiers are used with curly brackets ({ and }). You put two numbers between the curly brackets - for the lower and upper number of patterns. Both {}, +, \*, ? đều thuộc category 'quantifiers'. For example, to match only the letter a appearing between 3 and 5 times in the string ah, your regex would be /a{3,5}h/.

Sometimes you only want to specify the lower number of patterns with no upper limit. To only specify the lower number of patterns, keep the first number followed by a comma. For example, to match only the string hah with the letter a appearing at least 3 times, your regex would be /ha{3,}h/.

Sometimes you only want a specific number of matches. To specify a certain number of patterns, just have that one number between the curly brackets. For example, to match only the word hah with the letter a 3 times, your regex would be /ha{3}h/.

### Character Class Subtraction

The character class `[a-z-[aeiuo]]` matches a single letter that is not a vowel. In other words: it matches a single consonant. Without character class subtraction or intersection, the only way to do this is to list all consonants: `[b-df-hj-np-tv-z]`.

### Character Class Intersection

Character class intersection is supported by Java, ICU, JGsoft V2, and by Ruby 1.9 and later. It makes it easy to match any single character that must be present in two sets of characters. The syntax for this is `[class&&[intersect]]`. You can use the full character class syntax within the intersected character class.

## Negated character class

A `negated character class` is often **more appropriate** than the dot. Let’s illustrate with an example.

Suppose you want to match a double-quoted string. Sounds easy. We can have any number of any character between the double quotes, so `".*"` seems to do the trick just fine. The dot matches any character, and the star allows the dot to be repeated any number of times, including zero. If you test this regex on `Put a "string" between double quotes`, it matches `"string"` just fine. Now go ahead and test it on `Houston, we have a problem with "string one" and "string two". Please respond.`  
Ouch. The regex matches `"string one" and "string two"`. Definitely not what we intended. The reason for this is that the star `*` is **greedy**.

In the date-matching example, we improved our regex by replacing the dot with a character class. Here, we do the same with a negated character class. **Our original definition of a double-quoted string was faulty**. We do not want any number of any character between the quotes. We want any number of characters that are NOT double quotes OR newlines between the quotes. So the proper regex is `"[^"\r\n]*"`. If your flavor supports the shorthand `\v` to match any line break character, then `"[^"\v]*"` is an even better solution.

### Dealing with Japanese with Negated set

```json
"data":[
      {
         "name":"semeyez",
         "business_name":"EYEZ,INC.\t\t\t",
         "timezone":"Asia\/Tokyo",
         "timezone_switch_at":"2016-05-14T15:00:00Z",
         "country_code":"JP",
         "id":"18ce54bjjag",
         "created_at":"2016-05-16T01:37:23Z",
         "updated_at":"2026-06-25T01:00:39Z",
         "industry_type":"AGENCY",
         "business_id":"a5",
         "approval_status":"ACCEPTED",
         "deleted":false
      },
      {
         "name":"sem_eyez - EYEZ,INC.",
         "business_name":"EYEZ,INC.\t\t\t",
         "timezone":"Asia\/Tokyo",
         "timezone_switch_at":"2013-05-21T15:00:00Z",
         "country_code":null,
         "id":"18ce54jo9k0",
         "created_at":"2017-08-08T02:31:15Z",
         "updated_at":"2026-06-25T01:00:39Z",
         "industry_type":null,
         "business_id":"a5",
         "approval_status":"ACCEPTED",
         "deleted":false
      },
```

`/"name":"[^""]*?\\t[^""]*?"/gm`

`\w` không match japanese, ngoài ra japanese còn chứa các ký tự khác như （大阪）(khác với normal parentheses), 【公式】「古代ＤＮＡ－日本人のきた道－」 => cách tốt nhất là dùng negated set `[^""]`

`\t` (matching a literal tab) is different from `\\t` (match `\t`)

Switching from positive matching (`\w`) to a negated character set (like `[^...]`) is a total game-changer, especially when dealing with international languages like Japanese, Chinese, or Korean.

Instead of trying to define what a character is (which fails across different languages and alphabets), a negated character set defines what a character is NOT. Since a space or a quotation mark looks the same whether it's next to English or Japanese, it acts as a universal boundary.

The "Stay Inside the String" pattern: `[^"]` or `[^']` match anything that is not a quotation mark. This is the gold standard for scraping text inside attributes or JSON values. It guarantees your regex engine won't accidentally spill out over the closing quote and eat the rest of your file.  
Example: `"[^"]*"` matches a complete double-quoted string perfectly, regardless of the language inside.

The "Stay on the Current Line" pattern: `[^\n]` or `[^\r\n]` match any character except a newline or carriage return. By default, the wildcard dot (.) in many engines can accidentally match newlines if flags are turned on. Using `[^\n]*` forces your regex to strictly process data within a single, individual line.

The "Universal Word / Token" pattern: `[^[:space:]]` (Vim) or `[^\s]` (VS Code) match any character that is **not** a space, tab, or newline. This is your direct, bulletproof replacement for `\w`. Because a "space" is universal across all human languages, matching "anything that isn't a space" will perfectly capture Japanese words, English words, numbers, and symbols uniformly.  
Example (Tab between Japanese words): `[^\s]+\t[^\s]+`

The "HTML / XML Tag Confiner" pattern: `[^>]` match any character that is not a closing angle bracket. If you are parsing or cleaning up HTML/XML tags, using `.*` inside a bracket can accidentally match across multiple tags. Using `[^>]*` confines the engine to the current tag structure.  
Example: `<div[^>]*>` matches the opening `div` tag along with all of its internal classes and attributes, stopping exactly at the end of that specific tag.

The "CSV / Separated Values" pattern: `[^,]` (for CSV) or `[^\t]` (for TSV) match any character except a comma or a tab. Perfect for parsing data columns without pulling in a heavy CSV parsing library.  
Example: `([^,]*),([^,]*)` captures the first two columns of a comma-separated file cleanly.

## Capture Groups `()`

By placing part of a regular expression inside round brackets or parentheses `()`, you can group that part of the regular expression together. This allows you to apply a quantifier to the entire group or to restrict alternation to part of the regex.

Only parentheses can be used for grouping. Square brackets define a character class. Curly braces are used by a quantifier with specific limits.

Use Parentheses for Grouping and Capturing.

We can group an expression and use these groups to reference or enforce some rules. To group an expression, we enclose them in parentheses `()`.

- `(expr)`: Limits scope, groups elements, allows matches to be captured
- `(){}` capture groups with min-max
- `([AEae]l[- ])?` two character sets inside capture group and an optional `?` quantifier

### Reusing Patterns with Capture Groups

- When a match succeeds, every **set of parentheses** becomes a `capture group` that records the actual text that it matched.
  - Group number one (`$1`) will contain all the text inside the first `()` in the pattern.
  - Group zero (`$0`) always contains the entire regex match.

- `price: \$(\d+)` matches the string `price: $45` with:
  - `$0` (Entire Match): `price: $45`
  - `$1` (First Capture Group): `45` (only the part inside the parenthesis `\d+`)

Since parentheses can nest, how do you know which match is which? Easy—the matches arrive in the same order as the opening parentheses. There are as many captures as there are opening parentheses, regardless of the role (or lack of role) that each parenthesized group played in the actual matching. When a parenthe-sized group is not used (e.g., `Mu(')?ammar` when matched against `Muammar`), its corresponding capture is empty.

If a group is matched more than once, only the contents of the last match are returned. For example, with the pattern `(I am the (walrus|egg man)\. ?){1,2}` matching the text "I am the egg man. I am the walrus." There are two results, one for each set of parentheses:

1. I am the walrus.
2. walrus

Note that both capture groups actually matched twice. However, only the last text to match each set of parentheses is actually captured.

This is also know as referencing a group. Say you want to match a word that occurs multiple times.

The substring matched by the group is saved to a temporary "variable", which can be accessed within the same regex using a backslash and the number of the capture group (e.g. \1). Capture groups are automatically numbered by the position of their opening parentheses (left to right), starting at 1.

All the quantifiers including the star `*`, `+`, repetition {m,n} and the question mark ? can all be used within the capture group patterns. This is the only way to apply quantifiers on sequences of characters instead of the individual characters themselves.

---

**Non-capturing group `(?:)`**

Use the special syntax `(?:Value)` to group tokens without creating a capturing group. This is more efficient if you don’t plan to use the group’s contents.

In `Set(?:Value)?`, do not confuse the question mark in the non-capturing group syntax with the quantifier.  
The question mark and the colon after the opening parenthesis (`(?:`) are the syntax that creates a non-capturing group.  
The question mark after the opening parenthesis is unrelated to the question mark at the end of the regex. The final question mark is the quantifier that makes the previous token optional. This quantifier cannot appear after an opening parenthesis, because there is nothing to be made optional at the start of a group. Therefore, there is no ambiguity between the question mark as an operator to make a token optional and the question mark as part of the syntax for non-capturing groups, even though this may be confusing at first. There are other kinds of groups that use the (? syntax in combination with other characters than the colon that are explained later in this tutorial.

In the pattern `/(?:ha)-ha,(haa)-\1/g`, there are two groups. However, the first group reference we denote with `\1` actually indicates the second group, as the first is a non-capturing group.

`color=(?:red|green|blue)` is another regex with a non-capturing group. This regex has no quantifiers. The group is needed to keep the three alternatives together.

Nested groups

When you are working with complex data, you can easily find yourself having to extract multiple layers of information, which can result in nested groups. Generally, the results of the captured groups are in the order in which they are defined (in order by open parenthesis).

The nested groups are read from left to right in the pattern, with the first capture group being the contents of the first parentheses group, etc.

For the following strings, write an expression that matches and captures both the full date, as well as the year of the date.

### Use Capture Groups to search and replace

You can search and replace text in a string using .replace() on a string. The inputs for .replace() is first the regex pattern you want to search for. The second parameter is the string to replace the match or a function to do something.

You can also access capture groups in the replacement string with dollar signs ($).

## Backreferences

Within the regular expression, you can use the backreference `\1` to match the same text that was matched by the capturing group. `([abc])=\1` matches `a=a`, `b=b`, and `c=c`. It does not match anything else. If your regex has multiple capturing groups, they are numbered counting their opening parentheses from left to right.

The first parenthesis starts backreference number one, the second number two, etc.

---

Named Groups and Backreferences

If your regex has many groups, keeping track of their numbers can get cumbersome. Make your regexes easier to read by naming your groups. `(?<mygroup>[abc])=\k<mygroup>` is identical to `([abc])=\1`, except that you can refer to the group by its name. If this doesn’t work then try the Python-style syntax `(?P<mygroup>[abc])=(?P=mygroup)`. Though the Python-style named backreference uses parentheses as part of its syntax, it is not a group.

## Alternation `|`

The pipe character `|` (also called the "Or" operator) allows to specify that an expression can be in different expressions. Thus, all possible statements are written separated by the pipe sign `|`. This differs from charset `[abc]`, charsets operate at the character level while (`|`) alternatives are at the expression level.

For example, the following expression would select both "cat" and "rat": `/(c|r)at/g`.\
If you want to find either Penguin or Pumpkin in a string, you can use the following regex: `/P(engu|umpk)in/g`

For the `|` character, however, thingness extends indefinitely to both left and right. If you want to limit the scope of the vertical bar, enclose the bar and both things in their own set of parentheses. For example:

`I am the (walrus|egg man)\.` => matches either “I am the walrus.” or “I am the egg man.”. This example also dem-onstrates escaping of special characters (here, the dot).

If you want to search for the literal text `cat` or `dog`, separate both options with a vertical bar or pipe symbol: `cat|dog`. If you want more options, simply expand the list: `cat|dog|mouse|fish`.

Parentheses are the only way to stop the vertical bar from splitting up the entire regular expression into two options.

---

I already explained that the regex engine is eager. It stops searching as soon as it finds a valid match. The consequence is that in certain situations, the order of the alternatives matters. Suppose you want to use a regex to match a list of function names in a programming language: Get, GetValue, Set or SetValue.  
The best option is probably to express the fact that we only want to match complete words. We do not want to match `Set` or `SetValue` if the string is `SetValueFunction`. So the solution is `\b(Get|GetValue|Set|SetValue)\b` or `\b(Get(Value)?|Set(Value)?)\b`. Since all options have the same end, we can optimize this further to `\b(Get|Set)(Value)?\b`.

## Lookaround

`Lookaround` is a special kind of group. The tokens inside the group are matched normally, but then the regex engine makes the group give up its match and keeps only the result. Lookaround **matches a position**, just like anchors. It does not expand the regex match.

If we want the phrase we're writing to come before or after another phrase, we need to "lookaround". There are four types of lookaround.

Phần lookaround không được select. Nó chỉ dùng như điều kiện.

- lookahead = further to the right of the string.
- lookbehind = look further to the left side of the string

---

**Positive & Negative Lookahead: `(?=)` and `(?!)`**

Placed after the pattern that you want to match.

`/\d+(?=PM)/g` => match digit that have `PM` after them like `3PM`.

`q(?=u)` matches the `q` in `question`, but not in `Iraq` or `iraqi`. This is positive lookahead. The u is not part of the overall regex match. The lookahead matches at each position in the string before a u.

- `^.*?(?=java)` replaces with empty string => Remove anything before the word `java`.
    - `^` matches the start of line
    - `.*?` Matches any characters non-greedily (stops at the first match).
    - `(?=java)` A positive lookahead that checks for the word `java` without including java in what gets replaced/deleted.

---

Positive & Negative Lookbehind: `(?<=)` and `(?<!)`

Thêm dấu `<` vào Lookahead, placed before the pattern that you want to match.

`/(?<=\$)\d+/g` => match $"5" but not "1064"

### Lookahead

Lookaheads are patterns that tell JavaScript to look-ahead in your string to check for patterns further along. There are two kinds of lookaheads: positive lookahead and negative lookahead.

- A **positive lookahead** will look to make sure the element in the search pattern is there, but won't actually match it. A positive lookahead is used as (?=...) where the ... is the required part that is not matched.
- On the other hand, a **negative lookahead** will look to make sure the element in the search pattern is not there. A negative lookahead is used as (?!...) where the ... is the pattern that you do not want to be there. The rest of the pattern is returned if the negative lookahead part is not present.

### Lookbehind

Positive LookBehind (?<=): matches a group before the main expression without including it in the result. Example: /(?<=[tT]he)./g => matches every first character preceded by the word “the/The”

Negative Lookbehind (?<!): specifies a group that can not match before the main expression (if it matches, the result is discarded)

For example, we want to select the price value in the text. Therefore, to select only the number values that are preceded by $, we need to write the positive lookbehind expression (?<=) before our expression. Add \$ after the = sign inside the parenthesis.

For example, we want to select numbers in the text other than the price value. Therefore, to select only numeric values that are not preceded by $, we need to write the negative lookbehind (?<!) before our expression. Add \$ after the ! inside the parenthesis.

## Anchors

Anchors do not match any characters. They match a position before, after, or between characters. They can be used to “anchor” the regex match at a certain position.

In an earlier challenge, you used the caret character `^` inside a character set `[]` to create a negated character set in the form `[^thingsThatWillNotBeMatched]`. But **outside** of a character set, the caret is used to search for patterns at the **beginning of strings**.

- The caret `^` matches **the position before the first character** in the string. Applying `^a` to `abcaaa` matches the first `a`. `^b` does not match `abc` at all, because the `b` cannot be matched right after the start of the string (matched by `^`).
- `$` matches the position right after the last character in the string. `c$` matches the last `c` in `accbbc`, while `a$` does not match at all. (Trong `vim`, the motion `$` also jump to the end of current line).

If the multiline flag (`m`) is enabled, `^` will match the beginning of **each line instead of the whole string** => Dùng khi input là one "big string" containing multiple lines. Nếu không có (`m`) flag thì `^` chỉ match the beginning of the first line.

A regex that consists solely of an anchor can only find **zero-length matches**.

---

If you have a string consisting of multiple lines, like `first line\nsecond line` (where \n indicates a line break), it is often desirable to work with lines, rather than the entire string. Therefore, most regex engines discussed in this tutorial have the option (`m` flag) to expand the meaning of both anchors. `^` can then match at the start of the string (before the `f` in the above string), as well as after each line break (between `\n` and `s`). Likewise, `$` still matches at the end of the string (after the last `e`), and also before every line break (between `e` and `\n`).

If the multiline flag (m) is enabled, `$` will match the end of a line instead of the whole string.

`/html$/gm` find the "html" texts only at the end of the line.

**Examples:**

`/^[0-9]/gm` => match numbers at the beginning of a line

`/^http[^s].*/` => match "<http://httpstatus.io/>"; "http" at the beginning

In `^\d{5}$`, The `^` and $ match the beginning and end of the search text but do not actually correspond to characters in the text; they are **zero-width assertions.** These char-acters ensure that only texts consisting of exactly five digits match the regular expression—the regex will not match five digits within a larger string. The \d es-cape matches a digit, and the quantifier `{5}` says that there must be exactly five digit matches.

---

Permanent Start of String and End of String Anchors

`\A` only ever matches at the start of the string. Likewise, \Z only ever matches at the end of the string. These two tokens never match at line breaks. This is true in all regex flavors discussed in this tutorial, even when you turn on “multiline mode”.

JavaScript, `std::regex`, POSIX, and XPath do not support \A and \Z. You’re stuck with using the caret and dollar for this purpose.

### Word Boundaries

Additionally, there is a special metacharacter `\b` which matches the boundary between a word and a non-word character. It's most useful in capturing entire words (for example by using the pattern `\w+\b`).

`\s` khác `\b` ở chỗ là `\b` match position while `\s` match the actual character.

The metacharacter `\b` is an anchor like the caret `^` and the dollar sign `$` . It matches at a position that is called a **word boundary**. This match is **zero-length**.

- There are three different positions that qualify as word boundaries:
  1. Before the first character in the string, if the first character is a word character.
  2. After the last character in the string, if the last character is a word character.
  3. Between two characters in the string, where one is a word character and the other is not a word character.

Simply put: `\b` allows you to perform a “whole words only” search using a regular expression in the form of `\bword\b`. A **word character** is a character that can be used to form words. All characters that are not “word characters” are “non-word characters”.  
`\bword\b` match `word` but not `words` or "aword"

In most flavors, characters that are matched by the short-hand character class \w are the characters that are treated as word characters by word boundaries.

Since digits are considered to be word characters, `\b4\b` can be used to match a `4` that is not part of a larger number. This regex does not match `44 sheets of a4`. So saying “\b matches before and after an alphanumeric sequence” is more exact than saying “before and after a word”.

The underscore is also treated as a word character. So `\b` can only match at the start and the end of the strings `one_word` and `_underscore_`. So a regex engine’s idea of a word is actually closer to the concept of an identifier in programming languages.

The hyphen and the apostrophe  (`'`) are not treated as word characters. So `\b` can match before and after the apostrophe and the hyphen in `John's mother-in-law`. The regex `\b\w+\b` thus finds 5 words in this string: `John`, `s`, `mother`, `in`, and `law`.

`\B` is the negated version of `\b`. `\B` matches at every position where `\b` does not. Effectively, `\B` matches at any position between two word characters as well as at any position between two non-word characters.

The anchor `\b` matches at a word boundary. A word boundary is a position between a character that can be matched by \w and a character that cannot be matched by \w. \b also matches at the start and/or end of the string if the first and/or last characters in the string are word characters. \B matches at every position where \b cannot match.

---

The section [Looking Inside The Regex Engine](https://www.regular-expressions.info/wordboundaries.html) for word boundary is helpful to read!

## Free-Spacing & Comments

Many application have an option that may be labeled “free-spacing” or “ignore whitespace” or “comments” that makes the regular expression engine ignore unescaped spaces and line breaks and that makes the # character start a comment that runs until the end of the line. This allows you to use whitespace to format your regular expression in a way that makes it easier for humans to read and thus makes it easier to maintain.

In free-spacing mode, whitespace between regular expression tokens is ignored. Whitespace includes spaces, tabs, and line breaks. Note that only whitespace between tokens is ignored.  
`a b c` is the same as `abc` in free-spacing mode.  
But `\ d` and `\d` are not the same. The former matches  d, while the latter matches a digit.  
`\d` is a single regex token composed of a backslash and a “d”. Breaking up the token with a space gives you an escaped space (which matches a space), and a literal “d”.

Flavors differ in how they handle groups that are opened with multiple characters.  
`(?:group)` is always the same as `(?: gro up )` in free-spacing mode. But you should not put spaces in the middle of the `(?:` that opens the group.

---

Free-Spacing in Character Classes

A character class is generally treated as a single token.  
`[abc]` is not the same as `[ a b c ]`. The former matches one of three letters, while the latter matches those three letters or a space.

In other words: free-spacing mode has no effect inside character classes. Spaces and line breaks inside character classes will be included in the character class. This means that in free-spacing mode, you can use `\` or `[ ]` to match a single space. Use whichever you find more readable. The hexadecimal escape `\x20` also works, of course.

---

Comments in Free-Spacing Mode

Another feature of free-spacing mode is that the `#` character starts a comment. The comment runs until the end of the line. Everything from the `#` until the next line feed character is ignored.

Putting it all together, the regex to match a valid date can be clarified by writing it across multiple lines:

```txt
# Match a 20th or 21st century date in yyyy-mm-dd format
((?:19|20)\d\d)            # year (group 1)
[- /.]                     # separator
(0[1-9]|1[012])            # month (group 2)
[- /.]                     # separator
(0[1-9]|[12][0-9]|3[01])   # day (group 3)
```

---

Comments Without Free-Spacing

Many flavors also allow you to add comments to your regex without using free-spacing mode. The syntax is `(?#comment)` where “comment” can be whatever you want, as long as it does not contain a closing parenthesis. The regex engine ignores everything after the `(?#` until the first closing parenthesis. A line break does not end such a comment.

```txt
an(?#this is a comment
I can even add a line break)hao
```

The pattern above match `anhao`, the comment inside `(?#)` is ignored.

## `ripgrep`

find all lines have a word that contains `fast` followed by some number of other letters?

```bash
$ rg 'fast\w+' README.md
75:  faster than both. (N.B. It is not, strictly speaking, a "drop-in" replacement
119:### Is it really faster than everything else?
```

## adrepo

k

## References

[regexLearn.com](https://regexlearn.com/learn)

[RegexOne.com](https://regexone.com/)

Learn Regex on FreeCodeCamp

[regex101](https://regex101.com/) for testing, playground

[regexLearn](https://regexlearn.com/)

[rexegg.com](https://www.rexegg.com/) not basic, but advace

Jan Goyvaerts's excellent [regular-expression.info](https://www.regular-expressions.info/) page, more tutorial style
