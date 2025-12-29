# Chapter 7: Open files and Save Them to Disk

## 42. Open a File by its Filepaht Using `:edit`

- `:pwd` Print working directory
- `:edit %<Tab>` '%' is a shorthand for the filepath in active buffer and `<Tab>` will expand the symbol into path
- `:edit %:h<Tab>` ':h' modifier removes the filename from the path

## 43. Open a File by Its Filename Using `:find`

- `:find` find a file in path using its filename
- `:set path+=app/**` add stuff to path

## 44. Explore the File System with netrw

- `vim .` to open current dir in netrw
- `:edit .` to open netrw from vim
- `:edit %:h` / `:Explore` to open netrw in dir of current buffer
- `:Sexplore` open Explore in a split (horizontal)
- `:Vexplore` open Explore in a split (vertical)
- `<C-^>` switch back from netrw to previous edit buffer

## 45. Save Files to Nonexistent Directories

- `<C-g>` echoes name and status of current file
- `!mkdir -p %:h` create all dirs for current file path

## 46. Save a File as the Super User

- `:w !sudo tee % > /dev/null` write current buffer as sudo
