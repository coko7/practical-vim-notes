# Chapter 6: Manage Multiple Files

## Track Open Files with the Buffer List

### Meet the Buffer List

- `:ls` list loaded buffers
    - leading digit indicates buffer num
    - '%' symbol indicates visible buffers in window
    - '+' symbol indicates modified buffers (unsaved)
- `<C-^>` toggle between current and alternate files
- `:bnext` go to next buffer
- `:bprev` go to prev buffer
- `:bfirst` go to first buffer
- `:blast` go to last buffer

### Use the Buffer List

- `:buffer N` jump to buffer using num
- `:buffer {bufname}` jump to buffer using filepath (tab-completion)
- `:bufdo` execute cmd in all buffers

### Deleting Buffers

Deleting a buffer does not delete the corresponding file

- `:bdelete N1 N2 N3` deletes buffer using num
- `:N,M bdelete` deletes buffer N through M (range)

## Group Buffers into a Collection with the Argument List

Easier to manage than buffers list

- `:args` show the argument list
- `:args [arglist]` set contents of argument list (can open files)
    - works with filename
    - works with globs \* or \*\*
- `:argdo` execute cmd in all buffers

### Handle Hidden Buffers on Quit

- `:quit` quit current buffer
- `:write` write current buffer
- `:edit` reload buffer from disk
- `:qall` quit all buffers
- `:wall` write all buffers

## Divide Your Workspace Into Split Windows

A window is a viewport onto a buffer.

- `<C-w>s` split window horizontally
- `<C-w>v` split window vertically
- `:sp[lit] {file}` H split + load `{file}`
- `:vsp[lit] {file}` V split + load `{file}`
- `<C-w>w` cycle between windows
- `<C-w>hjkl` move between windows
- `:close` close active window or `<C-w>c`
- `:only` close all windows except active one or `<C-w>o`

## Organize Your Window Layouts with Tab Pages

A tab holds a collection of windows.

- `:lcd {path}` set cwd locally for current window (applies to cur win, not cur tab)
- `:windo lcd {path}` set cwd locally for all windows within tab
- `:tabedit {filename}` open new tab page
- `<C-w>T` move current window into its own tab
- `:tabclose` close cur tab and all windows in it
- `:tabonly` close all others tabs and keep only cur one
- `:tabn[ext] {N}` or `{N}gt` goto tab N
- `:tabn[ext]` or `gt` goto prev tab
- `:tabp[revious]` or `gT` goto prev tab
- `:tabmove [N]` rearrange tabs
    - if N = 0, cur tab moved to beginning
    - if N omitted, cur tab moved to end
