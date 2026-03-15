# General
- **Normal**: for moving around a file and making edits
- **Edit mode**: **Insert** or **Replace**
- **Command-line**: for running a command
- **Visual** (plain, line, or block): for selecting blocks of text
`<ESC>`: back to Normal mode
`v`: Visual mode to Normal mode
Insert mode with `i` (or `I`, `a`, `A`, `o`, `O`), Replace mode with `R`, Visual mode with `v`, Visual Line mode with `V`, Visual Block mode with `<C-v>`, Command-line mode with `:`
`u`: undo
`U`: undo whole line
`ctrl+r`: redo
`.`: repeat last editing command
`ctrl+g`: show position
# **command-line mode**
- `:q` quit (close window)
- `:w` save/write, `:w FILE` save as FILE
- `:wq` save and quit
- `:e {name of file}` open file for editing
- `:ls` show open buffers
- `:help {topic}` open help
    - `:help :w` opens help for the `:w` command
    - `:help w` opens help for the `w` movement
- `:sp`
- `ctrl+d` auto-complete
- `ctrl+W` switch screen
    
    ```Plain
    :<range_lower_bound>, <range_upper_bound>s/old/new/g
    :%s/old/new/gc sutstitute all files with prompt individually
    ```
    
    ```Plain
    :r FILENAME
    :r !COMMAND
    ```
    
    ```Plain
    ":set xxx" sets the option "xxx".  Some options are:
        'ic' 'ignorecase'   ignore upper/lower case when searching
        'is' 'incsearch'    show partial matches for a search phrase
        'hls' 'hlsearch'    highlight all matching phrases
         You can either use the long or the short option name.
    
    Prepend "no" to switch an option off:   :set noic
    ```
    
# **Movement**
```Plain
w,b,e,ge
^,$,0
h,j,k,l
ctrl+u,ctrl+d
G,gg,[[,]]
ctrl+o, ctrl+i
H,M,L
  
 <number>+<movement>: move <number> times
 
 To line number: :<line number> or <line number>G
```
# **Action**
```Plain
Delete: d+<Movement>
    line: dd
Change/Delete and swtich to insert mode: c+<Movement>
    line: cc
Copy: y+<Movement>
    line: yy
Paste: p
e.g: 7dw
```
```Plain
Replace char: r+<character>
x=dl
s=xi
```
```Plain
Case swap: ~
remove space between lines: J
```
# **Modifier**
```Plain
Around: a
Inside: i
e.g: 
change inside []: `ci[`
delete include (): `da(`
]})">'`
% match ]})" not >'`
```
# **Search**
```Plain
find chat at current line:
  forward: f<character>
  backward: F<character>
  navigate: ,;
search in current file:
	/<regex>
	navigate: nN
	backward search: ?<regex>
```
# Marks
```Plain
Add marks:
	m+[a-z]: current file only
	m+[A-Z]: global
Go to mark:
	`+[a-zA-Z]: precise lcation
	'+[a-zA-Z]: begining of the line
Commands:
	:marks (...)
	:delmark (...)
Next/Prev lowercase mark:
	]`, [`
```
# More
Vimuim(Chrome)
`H J K L`
`f`
`?`
VSCodeVim
[https://github.com/VSCodeVim/Vim](https://github.com/VSCodeVim/Vim)
`jj` insert mode to normal mode
`gd`
`gh`
- plugin
    
    vim-sneak
    
    vim-surround