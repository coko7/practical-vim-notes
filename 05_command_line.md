# Chapter 5: Command-Line Mode

- Commonly named `Ex commands`
- Behave similarly to Insert mode
- `:h ex-cmd-index`

## Commands

- `:[range]delete [x]` Delete specified lines [into register x]
- `:[range]yank [x]` Yank specified lines [into register x]
- `:[line]put [x]` Oyt the text from register x after the specified line
- `:[range]copy {address}` Copy the specified lines to below the line specified to {address}
    - does not use a register (unlike `yyp`)
- `:[range]move {address}` Move the specified lines to below the line specified to {address}
- `[range]join` Join the specified lines
- `[range]normal {commands}` Execute Normal mode {commands} on each specified lines
- `[range]substitute/{pattern}/{string}/[flags]` Replace occurences of {pattern} with {string} on each specified line
- `[range]global/{pattern}/[cmd]` Execute the Ex command [cmd] on all specified lines where the {pattern} matches
- `:edit` Edit the current file
- `:write` Write the whole buffer to the current file
- `:quit` Quit the current window
- `:tabnew` Open a new empty tab
- `:edit` Edit the current file
- `:write` Write the whole buffer to the current file
- `:quit` Quit the current window
- `:split` Split current window in two
- `:prev`/`:next` Interact with argument list
- `:bprev`/`:bnext` Interact with buffer list

## Ranges

- Ranges `{start_addr}[+n][,A{end_addr}[+n]]`
- Set of contiguous lines
- Addresses may be:
    - a number
    - a mark
    - a pattern

Symbols:
- `1` First line of the file
- `$` Last line of the file
- `0` Virtual line above first line of the file
- `.` Line where the cursor is placed
- `'m` Line containing mark m
- `'<` Start of visual selection
- `'>` End of visual selection
- `%` The entire file (shorthand for `:1,$`)

### Examples

- `:1` move cursor to first line
- `:$p` move cursor to last line and print it
- `:2,5y` yank lines 2-5
- `:.,$p` print from current line (`.`) to end
- `:%p` print all lines (same as `:1,$p`)
- `:'<,'>p` print current selection:
    - `'<` -> mark indicating first line of visual selection
    - `'>` -> mark indicating last line of visual selection
    - if ran from Normal mode, always act on the lines that most recently formed a Visual mode selection
- `:/<html>/,/<\/html>/p` uses pattern to print range starting on line containing `<html>` and end on line with `</html>`
    - start: `/<html>/`
    - end: `/<\/html>/`
- `{start}` and `{end}` addresses also work with offsets:
    - `:/<html>/+1,/<\/html>/-1p`:
        - start: `/<html>/+1`
        - end: `/<\/html>/i-1`

## Command-Line mode behaviours

- `<Tab>`/`<S-Tab>` cycle through autocomplete
- `<C-d>` print all options of autocomplete
- `<C-r><C-w>` copy the word under cursor
- `<C-r><C-a>` copy the WORD under cursor

## History

- Different history for searches `/` and Ex commands `:`
- Press `<Up>`/`<Down>` to move through history
    - Smart traversal since it will only explore history entries that match the already entered characters
- History persists when you quit Vim

### Command-Line Window

Problem: Command-Line mode behaves like Insert mode so motions are limited
Solution: Command-Line window allows to edit commands in a normal vim buffer

- `q:` Open Command-line window with history of Ex commands
- `q/` Open Command-line window with hisotry of searches
- `<C-f>` Switch from Command-line mode to Command-line window

- Execute a command on a line by pressing `<CR>`
    - The command is then executed in the context of active window (the window the cursor was originally in)
- The Command-Line window always steals the focus so you cannot move to other buffers except by closing it

## Run Commands in the Shell

- Prefix command with `!` to execute a shell command:
    - `:!ls` executes the shell ls command
    - `:ls` executes the Vim's built-in command
- '%' symbol is shorthand for current file name
- `:shell` start an interactive shell session (⚠️ does not work with Neovim)

### Using the Contents of a Buffer for Standard Input or Output

- `:read !{cmd}` puts the ouput of `{cmd}` into our current buffer
- `:write !{cmd}` uses the contents of the buffer as standard input for `{cmd}`

Do not confuse:
`:write !sh` pass buffer contents as standard input to `sh`
`:write ! sh` pass buffer contents as standard input to `sh`
`:write! sh` writes buffer contents to file 'sh' and overwrite any existing file (`!`)

### Filtering the Contents of a Buffer Through an External Command

- `:[range]!{cmd}` lines in `[range]` are passed to external command and cmd output will overwrite the selection
- `!{motion}` in normal mode drops into Command-Line and prepopulates the `[range]`. Examples:
    - `!G` opens a prompt with `:.,$!`

## Run Multiple Ex Commands as a Batch

- Put batch of commands in a `my-script.vim` file
- Each line in the file corresponds to a Command-Line line (`:` prefix on line is OPTIONAL). Examples:
```vim
global/href/join
vglobal/href/delete
%normal A: http://vimcasts.org
%normal yi"$p
%substitute/\v^[^\>]+\>\s//g
```
- Use `:source my-script.vim` to execute the script file (⚠️ does not work as is in Neovim)

### Source the Script to Change Multiple Files

Launch Vim with a wildcard to populate the argument list with all the files that match that pattern:
```console
$ vim vimcasts/*.html
```

- `:args` Print the arguments list (all files in this case)
- `:first`
- `:[count]next` Edit `[count]` next files
- `:argdo source my-script.vim` will execute commands from `my-script.vim` for all files
