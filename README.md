# Vim Cheat Sheet

## 1. Modes Overview
*Vim starts in Normal mode, where every key is a command. `Esc` returns to Normal mode from anywhere.*

| Mode                                      | What it does                    | Enter                     | Leave                              |
| :---------------------------------------- | :------------------------------ | :------------------------ | :--------------------------------- |
| [Normal](#2-normal-mode)                  | keys run commands               | `Esc`                     | any key below                      |
| [Insert](#3-insert-mode)                  | keys type text                  | `i`, `a`, `o` and friends | `Esc`                              |
| [Visual](#4-visual-mode)                  | select text, then act on it     | `v`, `V`, `Ctrl-v`        | `Esc`                              |
| [Command-line](#5-command-line-mode)      | type an Ex command such as `:w` | `:`, `/`, `?`             | `Enter` runs it, `Esc` abandons it |

* `i`: start typing (Insert mode)
* `Esc`: stop typing (back to Normal mode)
* `u`: undo
* `:wq`: save and quit
* `:q!`: quit without saving

---

## 2. Normal Mode
*The default mode. Every key is a command, and commands are like sentences.*

### 2.1 Motions
*A motion is a cursor movement. On its own it moves the cursor. After an operator (see 2.2) it marks the text from the cursor to where the motion would land. The region depends on where the cursor is. `dw` from the middle of a word deletes only the rest of it.*
* `h`, `j`, `k`, `l`: left, down, up, right
* `w`/`b`: move to start of next/previous **w**ord (letters, digits, underscore). Exception: `cw` acts like `ce`
* `W`/`B`: move by **W**ORD (anything but whitespace)
* `e`/`E`: move to the **e**nd of the word / WORD
* `0`/`^`/`$`: move to the start of the line / first non-blank character / end of the line
* `_`: first non-blank character of the line, line-wise (so `d_` deletes the whole line)
* `f{char}`/`F{char}`: **f**ind character on the current line (forward/backward)
* `t{char}`/`T{char}`: move **t**ill character on the current line (forward/backward)
* `;`/`,`: repeat the last `f` or `t` command (forward/backward)
* `{`/`}`: move to the previous/next blank line (paragraph)
* `gg`/`G`: go to the first/last line of the file
* `H`/`M`/`L`: go to **H**igh / **M**iddle / **L**ow part of the screen
* `Ctrl-u`/`Ctrl-d`: move half a screen **u**p / **d**own
* `%`: jump to matching bracket `()`, `{}`, `[]`
* `5j`, `3w`, ...: prefix any motion with a count to repeat it

### 2.2 The Vim Grammar: `operator` + (`motion` or `text object`)
Combine an **operator** (the verb) with a **motion** or **text object** (the noun).
Example: `d` (operator) + `w` (motion) = `dw` (delete to the next word). `c$` changes to the end of the line and `y}` yanks to the end of the paragraph.
While Vim waits for the motion it is in Operator-pending mode (`:help operator`).
You can add a number to repeat: `d2w` (delete 2 words).

#### 2.2.1 Operators (The Verb)
| Operator | Action |
| :--- | :----------------------------------- |
| `d` | **d**elete (acts like a "cut") |
| `c` | **c**hange (deletes and enters Insert mode) |
| `y` | **y**ank (copies) |
| `>`/`<` | shift right / left (e.g., `>}` shifts a paragraph) |
| `gu`/`gU`/`g~` | change to lowercase / uppercase / toggle case |

#### 2.2.2 Text Objects (The Other Noun)
*Any operator combines with any motion from 2.1. A text object is a region around the cursor with both ends chosen by Vim, such as the whole word or the contents of the quotes. It does not matter where in the object the cursor sits. `daw` from the middle of a word deletes the whole word and the space after it. Text objects cannot move the cursor, so they only work after an operator or in Visual mode.*
*Use `i` for "inner" (the contents) and `a` for "around" (the contents plus the delimiters or surrounding whitespace).*

| Text Object | Meaning | Example: `d` + Object | Example: `c` + Object |
| :---------- | :------------------------------------ | :-------------------- | :-------------------- |
| `iw`/`aw` | **i**nner / **a**round **w**ord | `diw` / `daw` | `ciw` / `caw` |
| `i"`/`a"` | **i**nner / **a**round `"` quotes | `di"` / `da"` | `ci"` / `ca"` |
| `i(`/`a(` | **i**nner / **a**round `()` parentheses | `di(` / `da(` | `ci(` / `ca(` |
| `i{`/`a{` | **i**nner / **a**round `{}` braces | `di{` / `da{` | `ci{` / `ca{` |
| `it`/`at` | **i**nner / **a**round **t**ag | `dit` / `dat` | `cit` / `cat` |
| `ip`/`ap` | **i**nner / **a**round **p**aragraph | `dip` / `dap` | `cip` / `cap` |

*`'`, `` ` ``, `[` and `<` work the same way as `"` and `(`.*

#### 2.2.3 Doubling the Operator (The Current Line)
*A doubled operator acts line-wise on the current line (the same as the `_` motion): `dd` (delete line), `cc` (change line), `yy` (yank line), `>>` (shift line). Counts work here too (e.g., `2dd` or `d2d` deletes 2 lines).*

### 2.3 Pasting & Simple Edits
*Commands that don't fit the main grammar.*
* `p`/`P`: **p**aste after/before the cursor (yanked lines go below/above the current line)
* `x`: delete character under the cursor
* `r{char}`: **r**eplace one character
* `~`: toggle case of the character under the cursor
* `J`: **j**oin the next line onto the current one

### 2.4 The Dot (.) Command: Repeat Your Last Change
*Repeats the last action that changed the text.*
* `.`: repeat the last change (e.g., `dw`, `ciw` or `x`)

### 2.5 Undo/Redo
*Works in Normal mode only, so press `Esc` first. In Visual mode `u` changes case instead.*
* `u`: **u**ndo the last change
* `Ctrl-r`: **r**edo the last undone change

### 2.6 Registers (Vim's Clipboards)
*Every delete, yank, and paste goes through a "register", a named clipboard. Macros live in the same registers.*
* `"`: prefix to specify a register
* `:reg`: list **reg**ister contents
* `"ay{motion}`: yank into register `a` (e.g., `"ayw` or `"ayy`)
* `"ap`: paste from register `a`
* `"+y{motion}`: yank to system clipboard (e.g., `"+yy` for the current line; needs a build with `+clipboard`)
* `"+p`: paste from system clipboard
* `"0p`: paste the last yank, ignoring any deletes since

### 2.7 Marks & Jumps
*Marks are bookmarks in the text. Vim also keeps a jump list of where you have been.*
* `ma`: set mark `a` at cursor (uppercase marks like `mA` work across files)
* `'a`: jump to the first non-blank character of the line with mark `a`
* `` `a ``: jump to exact cursor position at mark `a`
* `` '' ``: jump back to the line of the previous position
* `Ctrl-o`/`Ctrl-i`: go back / forward through the jump list

### 2.8 Macros: Recording and Replaying Actions
*Record any sequence of commands and replay it to automate repetitive tasks.*
* `q{a-z}`: start recording a macro into a register (e.g., `qa`); press `q` again to stop
* `@{a-z}`: execute the macro from that register (e.g., `@a`); counts work: `5@a`
* `@@`: repeat the last executed macro

### 2.9 Folding (Hiding/Showing Sections)
*Use folds to collapse large sections of code or text.*
* `zf{motion}`: fold a region (e.g., `zf}` folds a paragraph)
* `zo`: open (unfold)
* `zc`: close (fold)
* `za`: toggle fold
* `zR`/`zM`: open / close all folds

---

## 3. Insert Mode
*Keys type text. `Esc` returns to Normal mode.*

### 3.1 Entering Insert Mode (How to Type)
* `i`: **i**nsert text before the cursor
* `a`: **a**ppend text after the cursor
* `I`: **i**nsert at the beginning of the line
* `A`: **a**ppend at the end of the line
* `o`: **o**pen a new line below and enter Insert mode
* `O`: **o**pen a new line above and enter Insert mode
* `Esc`: exit Insert mode and return to Normal mode

---

## 4. Visual Mode

### 4.1 Making Selections
*Visual mode selects text. After selecting, you can apply an operator to the selection, like `d` (delete) or `y` (yank).*
* `v`: enter **v**isual mode (character-wise)
* `V`: enter **V**isual mode (line-wise)
* `Ctrl-v`: enter Visual block mode (for column-wise selection)
* `o`: jump to the **o**ther end of the selection
* `gv`: reselect the previous selection
* `viw`, `vi(`, ...: text objects extend the selection
* `I`/`A` (block mode): insert / append on every selected line
* `Esc`: exit Visual mode

---

## 5. Command-line Mode
*Ex commands start with `:`. Searches start with `/` or `?`. `Enter` runs the command, `Esc` abandons it.*

### 5.1 Saving & Quitting (How to Get Out)
* `:w`: **w**rite (save) the file
* `:q`: **q**uit the current window
* `:wq`: **w**rite (save) and **q**uit
* `ZZ` or `:x`: save (only if changed) and quit
* `:q!`: **q**uit without saving (discard changes)
* `:qa`: **q**uit **a**ll windows (`:qa!` to discard changes)

### 5.2 Search & Replace
* `/foo`: search forwards for `foo`
* `?bar`: search backwards for `bar`
* `n`/`N`: repeat search in the same/opposite direction
* `*`/`#`: search for the word under the cursor (forward/backward)
* `:s/foo/bar/g`: **s**ubstitute `foo` with `bar` for all matches (**g**) on the current line
* `:%s/foo/bar/g`: replace in the whole file (`%` here is the range "all lines", not the bracket motion)
* `:%s/foo/bar/gc`: replace with confirmation
* `:10,20s/foo/bar/g`: replace in lines 10 to 20 only

### 5.3 Workspace Management
*Managing files, tabs, and split windows. **Tabs** contain **Windows**, and **Windows** display **Buffers**.*

#### 5.3.1 Buffers (files)
*A **buffer** is a file loaded into Vim's memory: the text itself. You can have many files open in the background as buffers, even if you can't see them.*
*Each file on disk maps to one buffer. Some buffers have no file: scratch, help and terminal buffers.*
* `:e {file}`: **e**dit a file in a new buffer
* `:bn`: go to **n**ext **b**uffer
* `:bp`: go to **p**revious **b**uffer
* `:ls`: **l**i**s**t open buffers
* `:b {n}`: go to **b**uffer number `n` (from `:ls`)
* `:bd`: **b**uffer **d**elete (close buffer)

#### 5.3.2 Windows (splits)
*A **window** is a viewport on your screen that displays a buffer. Splitting the screen shows several files, or several parts of one file, at once.*
* `:sp [file]`: **sp**lit window horizontally (current buffer, or a file)
* `:vsp [file]`: **v**ertical **sp**lit window
* `Ctrl-w` + `h,j,k,l`: move to window left, down, up, right
* `Ctrl-w` + `w`: cycle through windows

#### 5.3.3 Tabs (workspaces)
*A **tab** is a collection of one or more windows. Use tabs to organise your screen into different workspaces (e.g., a "frontend" tab with HTML/CSS windows, and a "backend" tab with Python windows).*
* `:tabnew`: open a new **tab**
* `gt` or `:tabn`: **g**o to **t**ab **n**ext
* `gT` or `:tabp`: **g**o to **t**ab **p**revious
* `:tabclose`: close current tab
