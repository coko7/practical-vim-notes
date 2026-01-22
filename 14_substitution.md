# Chapter 13: Search

## 88. Meet the Substitute command

- `:[range]s[ubstitute]/{pattern}/{str}/[flags]`
- flags (`:h :s_flags`):
    - `g` (global): change all matches on line rather than stopping at first
    - `c` (confirm): prompts for confirmation before applying change
    - `n` (number): does not apply sub, simply returns count instead
    - `&`: reuse the same from flags as the last sub cmd

### Special Characters for the Replacement String

Help: `:h sub-replace-special` 

Chars:
- `\r`: insert carriage return
- `\t`: insert tab char
- `\\`: insert a single backslash
- `\1`: insert first submatch
- `\2`: insert second submatch (and so on, up to `\9`)
- `\0`: insert the entire matched pattern
- `&`: insert the entire matched pattern
- `~`: Use `{string}` from the previous invocation of `:sub`
- `\={Vim script}`: Evaluate `{Vim script}` expression; use result as replacement `{string}`

## 89. Find and Replace Every Match in a File

Use `%`:
- `:s/foo/bar`: replace first occurence of 'foo' by 'bar' on current line
- `:s/foo/bar/g`: replace all occurences on current line
- `:%s/foo/bar/g`: replace all occurences in entire file

## 90. Eyeball Each Substitution

Use `c` to confirm. Help: `:h :s_c`

## 91. Reuse the Last Search Pattern

- Leave `{search}` field blank to reuse last search query
    1. `/foo` searches for all occurences of 'foo' in file
    2. `:%s//bar/g` replace all 'foo' by 'bar` in file
        - same as `:%s/foo/bar/g`
- `<C-r>/` to insert last search query
    - example: `:%s/<C-r>//bar/g`

## 92. Replace with the Contents of a Register

- `:%s//<C-r>0/g`: insert content of register 0 (pass by value)
    - works well in most cases unless reg 0 contains some special chars or if its a multiline string
- `:%s//\=@0/g`: insert content of register 0 (pass by reference)
    - works in all cases because it will **evaluate** the content of register 0, therefore getting rid of potential special chars

- Comparison:
    1. By value: `:%s/Pragmatic Vim/Practical Vim/g`
    2. By ref:
    ```vimscript
    :let @/='Pragmatic Vim'
    :let @a='Practical Vim'
    :%s//\=@a/g
    ```
    - Both options do the same thing but will appear differently in the cmd history

## 93. Repeat the Previous Substitude Command

- `g&` is same as `:%s//~/&` (sub on entire file/reuse search/reuse same replacement string/reuse same flags)
    - Help: `:h g&`

- `gv`: go into visual mode and highlight last selection
- `:'<,'>&&`:
    - `'<,'>` -> current visual selection
    - `&` -> repeat last sub cmd (same as `:s`
    - `&` -> reuse flags from previous sub cmd

Caveat of `&` command is that despite repeating the last sub, it does not reuse the flags.
You can fix that with this useful remap:
```vimscript
nnoremap & :&&<CR>
xnoremap & :&&<CR>
```

## 94. Rearrange CSV Fields Using Submatches

*wip*
