# VIM Cheat sheet

This cheat sheet covers Vim from basic survival to its command grammar.

---

## Part 1: The Absolute Basics
*Everything you need to survive your first 5 minutes in Vim.*

### Insert Mode (How to type)
* `i`: **i**nsert text before the cursor
* `a`: **a**ppend text after the cursor
* `I`: like `i`, but **I**nsert at the beginning of the line
* `A`: like `a`, but **A**ppend at the end of the line
* `o`: **o**pen a new line below and enter insert mode
* `O`: like `o`, but **O**pen a new line above and enter insert mode
* `Esc`: exit insert mode and return to Command Mode

### Saving & Quitting (How to get out)
* `:w`: **w**rite (save) the file
* `:q`: **q**uit the current window/buffer
* `:wq`: **w**rite (save) and **q**uit
* `:q!`: **q**uit without saving (discard changes)

### Undo/Redo (How to fix mistakes)
* `u`: **u**ndo the last change
* `Ctrl-r`: **r**edo the last undone change

---

## Part 2: The Core of Vim - Command Mode Editing
*This is where Vim's power lies. Commands are like sentences.*

### Basic Navigation (The "Nouns" of Vim)
*These are the motions you can use alone, or combine with verbs.*
* `h`, `j`, `k`, `l`: left, down, up, right
* `w`/`b`: move to start of next/previous **w**ord (by alphanumeric chars)
* `W`/`B`: move by **W**ORD (by whitespace)
* `e`: move to the **e**nd of the current word
* `^`/`$`: move to the first non-whitespace character / end of the line
* `f{char}`/`F{char}`: **f**ind character on the current line (forward/backward)
* `t{char}`/`T{char}`: move **t**ill character on the current line (forward/backward)
* `;`/`,`: repeat the last `f` or `t` command (forward/backward)
* `gg`/`G`: go to the first/last line of the file
* `H`/`M`/`L`: go to **H**igh/ **M**iddle/ **L**ow part of the screen
* `ctl-u`/`ctl-d`: move half a screen **u**p/ **d**own
* `%`: jump to matching bracket `()`, `{}`, `[]`

### The Vim Grammar: `verb` + `noun`
Combine a **verb** (action) with a **noun** (motion or text object).
Example: `d` (verb) + `w` (noun) = `dw` (delete word).
You can add a number to repeat: `d2w` (delete 2 words).

#### Verbs (The Action)
| Verb | Action |
| :--- | :----------------------------------- |
| `d` | **d**elete (acts like a "cut") |
| `c` | **c**hange (deletes and enters Insert Mode) |
| `y` | **y**ank (copies) |
| `>`/`<` | indent right / left |
| `gu`/`gU` | change to lowercase / uppercase |

#### Nouns (The "Thing" to Act On)
*You can combine any verb above with any noun below.*

**1. Nouns as Motions (from the navigation section above)**

| Motion | Meaning | Example: `d` + Motion | Example: `c` + Motion |
| :----- | :--------------------- | :-------------------- | :-------------------- |
| `w` | to the next **w**ord | `dw` (delete word) | `cw` (change word) |
| `$` | to the end of the line | `d$` (delete to EOL) | `c$` (change to EOL) |
| `}` | to the next paragraph | `d}` (delete to para) | `c}` (change to para) |

**2. Nouns as Text Objects**
*Act on the "object" your cursor is inside. Use `i` for "inner" and `a` for "around".*

| Text Object | Meaning | Example: `d` + Object | Example: `c` + Object |
| :---------- | :------------------------------------ | :-------------------- | :-------------------- |
| `iw`/`aw` | **i**nner/ **a**round **w**ord | `diw` / `daw` | `ciw` / `caw` |
| `i"`/`a"` | **i**nner/ **a**round `"` quotes | `di"` / `da"` | `ci"` / `ca"` |
| `i(`/`a(` | **i**nner/ **a**round `()` parentheses | `di(` / `da(` | `ci(` / `ca(` |
| `i{`/`a{` | **i**nner/ **a**round `{}` braces | `di{` / `da{` | `ci{` / `ca{` |
| `it`/`at` | **i**nner/ **a**round **t**ag | `dit` / `dat` | `cit` / `cat` |

### Visual Mode: Making Selections
*Visual Mode selects text. After selecting, you can use a verb on the selection, like `d` (delete) or `y` (yank).*
* `v`: enter **v**isual mode (character-wise).
* `V`: enter **V**isual mode (line-wise).
* `Ctrl-v`: enter Visual **b**lock mode (for column-wise selection).
* `Esc`: exit Visual mode.

### The Dot (.) Command: Repeat Your Last Change
*One of Vim's most powerful commands. It repeats the last action that changed the text.*
* `.`: repeat the last change (e.g., a `dw` command, a `ciw` change, or an `x` command).

### Pasting & Simple Edits
*Commands that don't fit the main grammar.*
* `p`/`P`: **p**aste after/before the cursor
* `x`: delete character under the cursor
* `r{char}`: **r**eplace one character
* `dd`/`yy`: delete/yank the current line (a common shortcut)

---

## Part 3: Advanced Tools

### Search & Replace
* `/foo`: search forwards for `foo`
* `?bar`: search backwards for `bar`
* `n`/`N`: repeat search in the same/opposite direction
* `*`/`#`: search for the word under the cursor (forward/backward)
* `:s/foo/bar/g`: **s**ubstitute `foo` with `bar` **g**lobally on the current line

### Macros: Recording and Replaying Actions
*Record any sequence of commands and replay it to automate repetitive tasks.*
* `q{a-z}`: start recording a macro into a register (e.g., `qa`). Press `q` again while recording to stop.
* `@{a-z}`: execute the macro from the specified register (e.g., `@a`).
* `@@`: repeat the last executed macro.

### Workspace Management
*Managing files, tabs, and split windows. **Tabs** contain **Windows**, and **Windows** display **Buffers**.*

#### Multi-buffers (files)
*A **buffer** is a file loaded into Vim's memory: the text itself. You can have many files open in the background as buffers, even if you can't see them.*
*Each file on disk maps to one buffer. The only exception is a "scratch" buffer that you can create in memory which doesn't have a file yet.*
* `:e {file}`: **e**dit a file in a new buffer
* `:bn`: go to **n**ext **b**uffer
* `:bp`: go to **p**revious **b**uffer
* `:ls`: **l**i**s**t open buffers
* `:bd`: **b**uffer **d**elete (close buffer)

#### Windows (splits)
*A **window** is a viewport on your screen that displays a buffer. Splitting the screen shows several files, or several parts of one file, at once.*
* `:sp {file}`: **sp**lit window horizontally
* `:vsp {file}`: **v**ertical **sp**lit window
* `ctl-w` + `h,j,k,l`: move to window left, down, up, right
* `ctl-w` + `w`: cycle through windows

#### Tabs (workspaces)
*A **tab** is a collection of one or more windows. Use tabs to organize your screen into different workspaces (e.g., a "frontend" tab with HTML/CSS windows, and a "backend" tab with Python windows).*
* `:tabnew`: open a new **tab**
* `gt` or `:tabn`: **g**o to **t**ab **n**ext
* `gT` or `:tabp`: **g**o to **t**ab **p**revious
* `:tabclose`: close current tab


## Part 4: Power User Essentials

### Registers (Vim's Clipboards)
*Every delete, yank, and paste goes through a "register", a named clipboard.*

* `"`: prefix to specify a register
* `"` + `a` + `y` → yank into register `a`
* `"` + `a` + `p` → paste from register `a`
* `"+y`: yank to system clipboard
* `"+p`: paste from system clipboard

### Marks & Jumps
*Marks let you set "bookmarks" in text to return to later.*

* `ma`: set mark `a` at cursor
* `'a`: jump to beginning of line with mark `a`
* `` `a ``: jump to exact cursor position at mark `a`
* `` '' ``: jump back to previous position

### Advanced Search & Replace
* `:%s/foo/bar/g`: replace all `foo` with `bar` in the whole file
* `:%s/foo/bar/gc`: replace with confirmation
* `:10,20s/foo/bar/g`: replace in lines 10 to 20 only

---

## Part 5: Beyond the Basics

### Folding (Hiding/Showing Sections)
*Use folds to collapse large sections of code or text.*

* `zf{motion}`: fold a region (e.g., `zf}` folds a paragraph)
* `zo`: open (unfold)
* `zc`: close (fold)
* `za`: toggle fold

### The .vimrc (Customizing Vim)
*Your `.vimrc` file makes Vim "yours". A few useful options:*

```vim
set number        " show line numbers
syntax on         " enable syntax highlighting
set expandtab     " use spaces instead of tabs
set shiftwidth=4  " indent by 4 spaces
set ignorecase    " case-insensitive search
set smartcase     " ...unless uppercase is used
```

### Plugins (Extending Vim)
*Vim has a huge plugin ecosystem. Use a plugin manager (e.g., vim-plug):*

```vim
call plug#begin('~/.vim/plugged')
Plug 'tpope/vim-surround'
Plug 'preservim/nerdtree'
call plug#end()
```