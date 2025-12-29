# Chapter 10: Copy and Paste

## 60. Delete, Yank and Put with Vim's Unnamed Register

- Cutting and Yanking puts values in the unnamed register

## 61. Grok Vim's Register

- Prefix a command with `"{register}` to operate against a given register
- "Cut, Copy and Paste" is the equivalent of Vim's "Delete, Yank and Put"
- Some commands:
    - `:delete {register}` to cut current line
    - `:put {register}` to paste below current line

### The Unnamed Register ("")

- Addressed using `""`
- `""p` is same as `p`
- The `x`, `s` `d{motion}` and `y{motion}` all work against the unnamed register by default

### The Yank Register ("0)

- Addressed using `"0`
- The `y{motion}` yanks text into the unnamed register ("") as well as the yank register ("0)

### The Named Registers ("a-"z)

- 26 in total
- `"ay` will yank text and overwrite register a (lowercase letters)
- `"Ay` will yank text and append it to register a (uppercase letters)

### The Black Hole Register ("_)

- Addressed using `"_`
- Useful for deleting text without copying it to the unnamed register

### The System Clipboard ("+) and Selection ("*) Registers

- `"+y` and `"+p` for yanking into and pasting from the system clipboard (`:h quote+`)
- `"*y` and `"*p` for yanking into and pasting from the primary clipboard (specific to X11) (`:h quotestar`)
- No primary clipboard in Windows and Mac OS X, so both registers are the same

### The Expression Register ("=)

- Behaves differently from other registers

### Other Registers

- `"%` Name of the current file
- `"#` Name of the alternate file
- `".` Last inserted text
- `":` Last Ex command
- `"/` Last search pattern

## 63. Paste from a Register

- `gp` and `gP` behave like `p` and `P` but leave the cursor after the new text

## 64. Interact with the System Clipboard

- `<CTRL-SHIFT-v>` in insert mode to use the system paste command
- `:set pastetoggle=<f5>` to toggle the paste option
