# Chapter 12: Matching Patterns and Literals

## 73. Turn the Case Sensitivity of Search Patterns

- disable case sensitivity globally `:set ignorecase`
- adjust case sensitivty locally in search pattern:
    - ignore case sensitive with :`\c`
    - force case sensitive with :`\C`
    - both can be used anywhere in a pettern
    - to apply them to whole patter, add it at the end: `/foo bar\c`
- smartcase works by cancelling the ignorecase whenever uppercase letter is used:
    - `/foo bar baz` will be case insensitive
    - `/Foo bar baz` will be case sensitive

## 74. Use the `\v` Pattern Switch for Regex Searches

- Vim's syntax closer to POSIX than Perl

Some css:
```css
body { color: #3c3c3c; }
a { color: #0000EE; }
strong { color: #000; }
```
Match CSS color values:
- enable very magic search with `\v`
- With default search:      `#\([0-9a-fA-F]\{6}\|[0-9a-fA-F]\{3}\)`
- With very magic search:   `\v#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})`
- Even better with `\x`:    `\v#(\x{6}|\x{3})`

With very magic, `()|{` assume special meaning by default and do not need to be escaped

## 75. Use the `\V` Literal Switch for Verbatim Searches

- set *magic* and *nomagic* with `\m` and `\M`
- `\M` somewhat like `\V` except for some chars ('^' and '$')

```
The N key searches backward...
...the \v pattern switch (a.k.a. very magic search)...
```
Search literally for 'a.k.a':
- `/a\.k\.a\.` with normal search
- `/\Va.k.a` with verbatim search (very nomagic)
- Very nomagic => only backslash char `\` will retain special meaning

General rule:
- Use `\v` when using regexes
- Use `\V` when searching for literals

## 76. Use Parentheses to Capture Submatches

```
I love Paris in the
the springtime.
```
Find any pair of duplicate words: `\v<(\w+)\_s+\1>`
- `\v` enables very magic
- `<` and `>` match word boundaries
- `\_s` matches whitespace or line break (see `:h /\_` and `:h 27.8`)

Force vim to not assign match group when using parentheses with `%`:
- `/\v%(And|D)rew Neil` -> slightly faster than without `%`
- example with substitute to switch firstname and lastname around:
```
\v(%(And|D)rew) (Neil)
:%s//\2, \1/g
```

## 77. Stake the Boundaries of a Word

Find only word that match 'the': `/\v<the>`
- `\w` matches word chars (letters, nums, '_')
- `\W` inverse of `\w` (everything except word chars)
- `\zs` and `\ze` set start and end of the match (`:h \zs` for examples)
- `<` ~= `\W\zs\w` and `>` ~= `\w\ze\W`
- `*` performs `/\<foo\>` 
- `#` performs `?\<foo\>`
- `g*` and `g#` like `*` and `#` searches but without word delimiters

## 78. Stake the Boundaries of a Match

**pattern:** regex or literal typed into the search field
**match:** any text in the buffer that appears highlighted

Boundaries of a match same as start and end of pattern by default.
`\zs` and `\ze` can be used to crop the match (1. pattern fully matches, 2. `\z` to 'zoom' in on the match)

Example: *Match "quoted words"---not quote marks.*
- `/\v\zs"[^"]+\ze"`
- `/\v\"@<=[^"]+"@=` same as above (see help for more)

Trivia:
- `\zs` roughly equates to Perl's *positive lookbehind*
- `\ze` roughly equates to Perl's *positive lookahead*

## 79. Escape Problem Characters

[http://vimdoc.net/search?q=/\\](http://vimdoc.net/search?q=/\\)

Matching the above URL:
- Forward search:
    - Issue: `/\Vhttp://vimdoc.net/search?q=/\\` -> only highlights `http:`
        - `/` is the search field terminator in both `\v` or `\V` modes
    - Solution: escape the '/': `/\Vhttp:\/\/vimdoc.net\/search?q=\/\\`
- Backward search: 
    - Issue: `?\Vhttp://vimdoc.net/search?q=/\\` -> only highlights `http://vimdoc.net/search`
        - `?` is the search field terminator in both `\v` or `\V` modes
    - Solution: escape the '?': `?http://vimdoc.net/search\?q=/\\`

Backslash always needs to be escaped when trying to match it literally (in both `\v` and `\V`)

### Escape Characters Programmatically

Handy way to avoid manually escaping characters:
1. Copy the URL to a register: `"uyi(` (copies to reg 'u')
2. Press `/` or `?` to bring the search prompt
3. Enter `\V` and then type `<C-r>=` to switch to the expression register prompt
4. Then type `=escape(@u, getcmdtype().'\')` and press Enter
5. This will evaluate the expression and insert the return value into the search prompt

Very useful to search for URLs

Trivia on search field terminators:
- search field terminators are needed, ex:
    - `/vim/e<CR>` positions the cursor at the end of any match rather than the start
