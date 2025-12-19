# Regex Examples

## Numeric Ranges

Since regular expressions deal with text rather than with numbers, matching a number in a given range takes a little extra care.

You can’t just write `[0-255]` to match a number between 0 and 255. Though a valid regex, it matches something entirely different. [0-255] is a character class with three elements: the character range 0-2, the character 5 and the character 5 (again). This character class matches a single digit 0, 1, 2 or 5, just like `[0125]`.

The regex `[0-9]` matches single-digit numbers 0 to 9. `[1-9][0-9]` matches double-digit numbers 10 to 99.

## Floating Point Numbers

As an example, we will try to build a regular expression that can match any floating point number. Our regex should also match integers and floating point numbers where the integer part is not given.

When creating a regular expression, it is more important to consider what it should not match, than what it should.