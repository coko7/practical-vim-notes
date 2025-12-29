# Chapter 11: Macros

## 65. Record and Execute a Macro

- `@@` repeat last invoked macro
- execute macros in series or in parallel:
    - **in series:** will stop at the first error
    - **in parallel:** will skip errors and continue executing on the next lines
- if executing macro for all lines, better use a big count like `100000@a`, it will stop when it reaches the end anyway
- execute macros in parallel:
    - use visual mode to highlight
    - run `:'<,'>normal @a`
- append to a macro with `qA`
- run macro in all buffers: 
    - run in parallel: `:argdo normal @a` or `:bufdo normal @a`
    - run in series: `qA` -> `:next` -> `q` then `22@a`
- `:wnext` same as `:write` followed by `:next`

### Macro with dynamic value

Example of a macro that will work with a dynamic value:
Works in Neovim:
```vim
:let i=1
qa
I<C-r>=i<CR>)<Esc>
:let i+=1
q
```
To execute in parallel:
```vim
jVG
:'<,'>normal @a
```

This macro inserts number `X)` at start of each line in selection.
Simpler alternative is to go into visual-block, insert 1 at the start of each line and then `g<C-a>` to increment gradually.

### Edit macros

- put content of macro from register `:put a`
- put selection back into macro register:
    - `"add` / `:d a` puts entire line in register 'a'
    - `0"ay$dd` will put entire line in register 'a' without new '<CR>'
- run substitute on register directly:
```vim
:let @a=substitute(@a, '\~', 'vU', 'g')
```
