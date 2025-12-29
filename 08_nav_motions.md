# Chapter 8: Navigate Inside Files with Motions

## 47. Keep Your Fingers on the Home Row

## 48. Distinguish Between Real Lines and Display Lines

- `gj` Down on display line
- `gk` Up one display line
- `g0` To first char of display line
- `g^` To first nonblank of display line
- `g$` To end of display line

## 49. Move Word-Wise

- `ge` Backward to end of prev word
- word vs WORD

## 50. Find by Character

- `f,dt.` kinda finger macro

## 51. Search to Navigate

Search works like a motion for operators:
- `d/ge<CR>` Operator-pending mode with delete and search for `ge` string. Then delete up to the search

## 52. Trace Your Selection wiht Precision Text Objects

- Using text objects like `daw` or `cit`

## 53. Delete Around, or Change inside

## 54. Mark Your Place and Snap to it

Max of 26 marks per buffer.

- `m{a-zA-Z}` marks current cursor location with letter:
    - Lowercase marks are local to buffer
    - Uppercase marks are globally accessible
- `'{mark}` moves to the line where mark was set
- **`{mark}** moves the cursor to exact position where mark was set

Automatic marks:
- **``** or `<C-o>` Position before the last jump within current file
- **`.** Location of last change
- **`^** Location of last insertion
- **`[** Start of last change or yank
- **`]** End of last change or yank
- **`<** Start of last visual selection
- **`>** End of last visual selection

## 55. Jump Between Matching Parentheses

- `%` jump between opening and closing sets of parentheses: ({[]})
- Changing a pair of parenthesis:
    - Example:
    ```json
    ["London", "Berlin", "New York"]
    ```
    - Solution: **f[%r}``r[**

Check **matchit** with `:h matchit-install`
Check **Surround.vim** plugin
