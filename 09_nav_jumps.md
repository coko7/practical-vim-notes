# Chapter 9: Navigate Between Files with Jumps

## 56. Traverse the Jump List

- `<C-o>` Go backward in Jump list
- `<C-i>` Go forward in Jump list
- `:jumps` Inspect jump list
- Any command that changes the active file for the current window is a jump:
    - `:edit` to open a new file
    - `[count]G` to jump to a line
    - sentence-wise and paragraph-wise motions
    - and more
- ⚠️ Not a jump "short-range motions":
    - Move one line up/down
    - Character-wise and word-wise motions
- One jump list per window

## 57. Traverse the Change List

- `u<C-r>` Undo + redo has the side effect of moving cursor to last change pos
- `g;` Go backward in Change list
- `g,` Go forward in Change list
- `:changes` Inspect change list

### Marks for the Last Change

- **`.** references the position of the last change
- **`^** references the position of the last time Insert Mode was stopped
- `gi` Insert text at same pos as Insert Mode was stopped last time
    - equivalent of **`^i**

## 58. Jump to the Filename Under the Cursor

- `gf` Jump to the filename under the cursor

### Specify a File Extension

- `:set suffixesadd+=.rb` specify ruby file ext to make `gf` smarter

### Specify the Directories to Look Inside

- `:set path?` to inspect the contents of the path

## 59. Snap Between Files Using Global Marks

- `m{A-Z}` to create a global mark (via uppercase letter)
- `:vimgrep` grep in files and set error list to result
    - `:cnext` and `:cprev` to traverse the list
