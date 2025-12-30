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
- find any pair of duplicate words: `\v<(\w+)\_s+\1>`
    - `\v` enables very magic
    - `<` and `>` match word boundaries
    - `\_s` matches whitespace or line break (see `:h /\_` and `:h 27.8`)

- force vim to not assign match group when using parentheses with `%`:
    - `/\v%(And|D)rew Neil` -> slightly faster than without `%`
    - example with substitute to switch firstname and lastname around:
    ```
    \v(%(And|D)rew) (Neil)
    :%s//\2, \1/g
    ```

## 77. Stake the Boundaries of a Word
