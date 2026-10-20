# Key Mapping

## Basics

`n` Normal mode map. Defined using `:nmap` or `:nnoremap`.
`i` Insert mode map. Defined using `:imap` or `:inoremap`.
`v` Visual and select mode map. Defined using ':vmap' or ':vnoremap'.
`x` Visual mode map. Defined using `:xmap` or `:xnoremap`.
`s` Select mode map. Defined using `:smap` or `:snoremap`.
`c`  Command-line mode map. Defined using `:cmap` or ':cnoremap'.
`o`  Operator-pending mode map. Defined using ':omap' or ':onoremap'.

`<Space>`  Normal, Visual and operator pending mode map. Defined using `:map` or `:noremap`.
`!`  Insert and command-line mode map. Defined using `map!` or `noremap!`.

always use `noremap` or its relatives (e.g. `vnoremap`) unless you have an explicit reason not to (e.g. when working with `<Plug>` or `<SID>` mappings, which are meant to be remapped

---

`noremap = true` (No Recursive Mapping) prevents recursion / cascading remaps: It ensures that the target command (`rhs`) executes Vim's built-in behavior rather than being re-interpreted by any other custom keybindings you may have defined.

Example: If you map `a` to `b`, and then map `x` to `a`:

- With `noremap = true`: Pressing `x` triggers the default Vim behavior of `a` (append text).
- Without `noremap = true` (recursive): Pressing `x` would trigger `a`, which triggers `b`.

In almost all custom mappings, `noremap = true` is best practice to prevent unexpected loops or conflicts.

## global variable table

[vim.g](https://neovim.io/doc/user/lua/#vim.g) 



