# Chapter 13: Search

## 80. Meet the Search Command

- Toggle wrapscan: `:h wrapscan`
- `/<CR>` or `?<CR>` to repeat last search
- `gn` and `gN` to enable char-wise Visual Mode and select next/previous match
    - if already on a match, this will enable visual mode for the current match
- Navigate through search history with `<Up>`/`<Down>`

## 81. Highlight Search Matches

- `:h hlsearch` to manage highlighting on search

## 82. Preview the First Match Before Execution

- `:h incsearch` to manage incremental search
- `<C-r><C-w>` to autocomplete search field using remainder of current preview match

## 83. Offset the Cursor to the End of a Search Match

- Search puts cursor on first char of a match by default
- See `:h search-offset` to configure a search offset
- Example:
    ```txt
    Aim to learn a new programming lang each year.
    Which lang did you pick up last year?
    Which langs would you like to learn?
    ```
    Task: change all occurences of 'lang' to 'language'
    Solution 1: substitute
    Solution 2:
        - `/lang<CR>`, then `eauage<Esc>` to change occurence from 'lang' to 'language'
        - Use dot command to repeat `ne.`
        - Does not work for 'langs' as it creates 'langsuage'
    Solution 3:
        - `/lang/e<CR>` to search AND place the cursor at the end of the match
        - `auage<Esc>` and `n.` to repeat
        - Now all matches have been updated properly
- `//e<CR>` will repeat last search and place cursor at the end of matches

## 84. Operate on a Complete Search Match

```rb
class XhtmlDocument < XmlDocument; end
class XhtmlTag < XmlTag; end
```
Suppose we want 'XHTML' and 'XML' (uppercase)

Try:
- `/\vX(ht)?ml\C<CR>`
    - `\C` enforces case sensitivity
- `gUgn`
    - `gU{motion}` updates selection to uppercase
    - `gn` goes to next match and enables char-wise selection
- `.` to repeat the last action

Trying the same thing with a case-insensitive search `\c` will result in a slightly different behavior
- It still works but requires `n.` instead of simply `.`
- That's because the case-insensitivity will match the updated match (uppercase) after applying the change and wont jump to the next one automatically

## 85. Create Complex Patterns by Iterating upon Search History

- `q/` to bring the command-line window
- `c%` to replace parentheses and inside

```txt
This string contains a 'quoted' word.
This string contains 'two' quoted 'words.'
This 'string doesn't make things easy.'
```

Replace all quoted strings single quotes by double quotes
- `\v'(([^']|'\w)+)'`
- `:%s//"\1"/g` (subtitute with empty search pattern '//' repeats the last search)

## 86. Count the Matches for the Current Pattern

### In Neovim

**ℹ️ Neovim already count matches when searching (at bottom)**

### In Vim

Example:
```js
var buttons = viewport.buttons;
viewport.buttons.previous.show();
viewport.buttons.next.show();
viewport.buttons.index.hide();
```

How to do it in plain vim:
- `/\<buttons\>`
- `:%s///gn` 
    - '//' -> reuse last search
    - 'n' flag suppresses usual behavior of subtitute to simply count matches instead

With vimgrep:
- `/\<buttons\>`
- `:vimgrep //g %` -> populates the quick fix list with each match in current buffer
    - '//' -> reuse last search
    - '%' -> expands to filepath of current buffer
- can now use `:cnext` and `:cprev` to move between matches (`n` and `N` still work)

## 87. Search for the Current Visual Selection

- `*` searches occurences of word under cursor
- `:vmap X y/<C-R>"<CR>` (see `:h visual-search`)
    - problems with '.' and '*'
    - solution: *(might want to use lua instead to achieve the same in neovim)*
    ```vim
    xnoremap * :<C-u>call <SID>VSetSearch('/')<CR>/<C-R>=@/<CR><CR>
    xnoremap # :<C-u>call <SID>VSetSearch('?')<CR>?<C-R>=@/<CR><CR>

    function! s:VSetSearch(cmdtype)
      let temp = @s
      norm! gv"sy
      let @/ = '\V' . substitute(escape(@s, a:cmdtype.'\'), '\n', '\\n', 'g')
      let @s = temp
    endfunction
    ```
