# Chapter 3: Insert Mode

Insert mode commands:
- `<C-h>` delete back one character (backspace)
- `<C-w>` delete back one word
- `<C-u>` delete back to start of line
- `<Esc>` switch to Normal mode
- `<C-[>` switch to Normal mode
- `<C-o>` execute one command, return to normal mode
- `<C-r>{register}` paste from register
    - can be slow because Vim inserts text from the register as if typed one char at a time
- `<C-r><C-p>{register}` smarter paste, inserts text literally and fixes indentation
- `<C-r>=` evalute an expression in a prompt and paste result at cursor pos
- `<C-v>{code}` insert character by decimal code
- `<C-v>u{code}` insert character by hexadecimal code
- `<C-v>{nondigit}` insert nondigit literally
- `<C-k>{char1}{char2}` insert character represented by `{char1}{char2}` digraph

Normal mode:
- `R` switch to Replace mode
- `gR` switch to Virtual Replace mode
- `r` overwrite single character before switching back to Normal mode (Replace)
- `gr` overwrite single character before switching back to Normal mode (Virtual Replace)
