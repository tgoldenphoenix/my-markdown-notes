# Column, Grid & Flexbox notes

## Flexbox

Block elements có `width: auto;` and will fill up 100% width of its container. Flex items have their "ideal width" which means "as small as it can get to (all text in one single line, no wrapping) while keeping everything on one line" giống với khi set block elements to have `width: max-content;`.

When set `display: flex;` thì `gap: 0;`;  `flex: 0 1 auto` so there is no grow, flex items width là flexbox's "ideal width".

When set `flex-basis: auto; (default)` the flex items' widths are flexbox's "ideal width". There is no line-wrapping for the content.

Set `flex: 1;` changes two values: `flex-grow: 0 -> 1;` (flex items now grows evenly) and `flex-basis: auto -> 0%;` (making even columns). `flex-shrink: 1;` remains unchanged.
`flex-basis: 0` is like the `width: min-content`

`flex-basis: 300px;` sets all flex items' initial width to 300px

There are three ways to create even columns in flexbox:

1. `flex: 1;`
2. `width: 100%;`
3. `flex-basis: 100%;`
