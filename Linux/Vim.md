1. Start vim by typing **vim filename**
2. To insert text press **i** or **o** (drops one level down below last line of text)
3. Now start editing text. Add new text or delete unwanted text.
4. Press **Esc** key and type **:w** to save a file in vim.
5. One can press **Esc** and type **:wq** to save changes to a file and exit from vim
6. Another option is to press **:x**

**Save the file under a different name:**

- Press **ESC** key.
- Type **:w My_fileName_here**
- Hit **Enter** key.

### Some common vim commands

```bash
Press the <ESC> (escape) key to ensure you‟re in normal mode, then:

:q! quits without saving

:wq saves and quits (write quit)

x deletes individual characters

i inserts text

dw deletes to the end of a word (d2w deletes two words, d3w deletes three words, etc.)

d$ deletes to the end of a line

dd deletes an entire line (2dd deletes two lines, 23dd deletes 23 lines, etc.)

u undoes the last command

U fixes an entire line

<CTRL>R redoes the command

v + i + w selects whole word

p puts the last deletion after the cursor

r replaces the character under the cursor

cw is the “change word” command, that deletes the word (from the cursor to the right) and places you in “insert” mode

c$ is the “change line” command, that deletes the line (from the cursor to the right) and places you in “insert” mode

<CTRL>g shows your location in a file

<SHIFT>G moves to the end of the file, [number]<SHIFT>G moves to the line number specified in the command, for
example 1<SHIFT>G moves to line #1.

/[search term] searches forward through a file for the search term. For example, “/apache” will search for the
next instance of the word “apache” in the file

?[search term] searches backwards through a file for the search term. For example, “?apache” will search for
the last instance before the cursor of the word “apache” in the file

:s/[old]/[new] will replace the next instance of “old” with “new”. For example, :s/blue/red will replace the
next instance of “blue” with “red”.

:s/[old]/[new]/g will replace the every instance of “old” on the current line with “new”. For example, :s/blue/red
will replace the every instance of “blue” with “red”.

:#,#s/[old]/[new]/g will replace every instance of “old” with “new” in the range of lines specified with the #
sign.

:! allows you to execute external commands

:set nu turns on line numbering

:nohlsearch turns off highlighting of search terms
```



## Quit Vim without saving the file

**WARNING!** The following command exits the vim text editor, **DISCARDING** any changes you have made.

Finally, we can exit or quit the vim text editor without saving the changes to our file. All you have to do is:

- Press **Esc**
- Type **:q!**
- Hit **Enter** key

### Adding and removing text

- **Insert** new text **before** the cursor: `i <type inserted text> <ESC>`
- **Append** new text **after** the cursor: `a <type appended text> <ESC>`
- **Append** new text at the **end of the line**: `A <type appended text> <ESC>`
- Go to the end of file: Shift + g
- Go to start of file: Shift + g + g
- **Delete** the character at the **cursor**: `x`
- **Delete** from the cursor to the **end of a line**: `d$`
- **Delete** / **Cut** a whole **line**: `dd`
- To **put** back text that has just been deleted: `p`.

> This puts the deleted text AFTER the cursor (if a line was deleted it will go on the line below the cursor).

- To open a **line below** the cursor and start Insert mode: `o`
- To open a **line above** the cursor and start Insert mode: `O`

### Save file

`:w FILENAME` which **writes** the current Vim file to disk with the name FILENAME.

### Navigate throughout file

- To **move** to the **start of the line** use a zero: `0`
- To display your **location** in the file and the file status: `CTRL-G`
- To **move** to the **end of the file**:`G`
- To **move** to a **specific line**: `<number> G`
- To **move** to the **first line** in the file: `gg`
- To **move** to the **end of the word**: `e`

### Fix a typo

- To **undo** previous actions**:** `u`(lowercase u)
- To undo the undo’s (**Redo**): `CTRL-R`

### Find matching brackets

- Typing `%`while the cursor is on a **(**,**)**,**[**,**]** or **{**,**}** goes to its **match**.

### Search & replacements

- Typing `/` followed by a phrase **searches forward** for the phrase.
- Type `n`to get the **next** occurrence or `N`to return to the **previous** one.
- To **substitute all occurrences** in the file type : `%s/<old>/<new>/g`
- To **ask for confirmation** each time add `c` : `%s/<old>/<new>/gc`

```bash
:%s/Windows/Linux/g # replace Windows with Linux globally
```

### Split window for two files

```bash
# open new file while editing another one horizontally
:sp /path/to/file
:split /path/to/file

# split vertically
:vs /path/to/file
:vsplit /path/to/file

# to move pointer from one file to another
CTRL + w + w
```

### Vim config file

```bash
vim ~/.vimrc
# example - add line numbers
set number
```



### Copying & pasting

- Visually **select** text: `v <motion, either number or hjkl>`.
- **Yank** (**copy**) text: `y`
- **Put** (**paste**) text: `p`
- For a **line**, use `dd` cut a line, and `yy` copy that line.

### Execute commands on the terminal without closing Vim

- To **execute a command**: `:!<command>`
- Some useful examples -(Unix)- are:

`:!ls` shows a directory listing.
`:!rm FILENAME` removes file FILENAME.

### Help window

- **Open** a **help window**, type `:help`or press `<F1>` or `<HELP>`
- Jump to **another window**, type `CTRL-W CTRL-W`
- **Close** the **help window**, type `:q`

### IntelliSense

- press `CTRL-D` to see **possible completions**.
- Press `<TAB>` to **use one completion**.

### Open files in tabs

1. Open file1: `$ vim file1`

\2. Type, `:tabedit file2` , to to edit file2 in a new tab.

\3. Type, `:tabedit file3` , to to edit file3 in a new tab.

You can also use: `$ vim -p file1 file2 file3`

### To navigate between tabs

- Next tab: `:tabn`
- Previous tab: `:tabp`
- Go to tab i: `<i>gt` e.g: Go to tab 2-> `2gt`
- First Tab: `:tabfirst`
- Second Tab: `:tablast`
- List all the tabs: `:tabs`

### Close Tabs

- Close a current tab: `:tabclose`
- Close all expect current: `:tabonly`

### Save the session and **restore** it

1. Return to our lovely terminal.
2. Press `Esc` to enter the command mode.
3. Type, `:mksession work.vim` . Now, our current session of open tabs is stored in a file named`work.vim` .
4. Close all the tabs.
5. Now, run `$ vim -S work.vim` and Wow! we have restored our session with all the tabs open!

**How to copy lines in Vim**

**Yank (or cut) and Paste Multiple Lines**

1. Put your cursor on the top line.
2. Use shift+v to enter visual mode.
3. Press 2j or press j two times to go down two lines.
4. (Or use v2j in one swift ninja-move!)
5. Press y to yank or x to cut.
6. Move your cursor and use p to paste after the cursor or P to paste before the cursor.

### Search and Replace

```bash
# search for corona and replace with covid
:%s/corona/covid

# to replace on all the lines (globally)
:%s/corona/covid/g

# to replace with nothing (will delete the words)
:%s/corona//g
```

### How do I copy text from Vim?

**In vim command mode press v , this will switch you to VISUAL mode.** **Move the cursor around to select the text or lines you need to copy.** **Press y** , this will copy the selected text to clipboard. Go to any external application and CMD + v to paste.

**Key Command Mode:**  

| **gg** | **To go to the beginning of the page**      |
| ------ | ------------------------------------------- |
| G      | To go to end of the page                    |
| w      | To move the cursor forward, word by word    |
| b      | To move the cursor backward, word by word   |
| 5w     | To move the cursor forward to 5 words (SW)  |
| 5b     | To move the cursor backward to 5 words {SB) |
| u      | To undo last change (word)                  |

| **U**      | **To undo the previous changes (entire line)**               |      |      |
| ---------- | ------------------------------------------------------------ | ---- | ---- |
| **Ctrl+R** | **To redo the changes**                                      |      |      |
| **VY**     | **To copy a line**                                           |      |      |
| **5yy**    | **To copy 5 lines (Syy or 4yy)**                             |      |      |
| P          | **To paste line below the cursor position**                  |      |      |
| **p**      | **To paste line above the cursor position**                  |      |      |
| **dw**     | **To delete the word letter by letter {like Backspace}**     |      |      |
| **X**      | **To delete the world letter by letter (like DEL Key)**      |      |      |
| **dd**     | **To delete entire line**                                    |      |      |
| **5dd**    | **To delete 5 no. of lines from cursor position{Sdd)**       |      |      |
| /          | **To search a word in the file** **(slash). Press "n" to move to next found item** |      |      |

**Extended Mode: ( Colon Mode)**  

Extended Mode is used for save and quit or save without quit using "Esc" Key with":"  

| Esc+:w       | To Save the changes                                |      |      |
| ------------ | -------------------------------------------------- | ---- | ---- |
| Esc+:q       | To quit (Without saving)                           |      |      |
| Esc+:wq      | To save and quit                                   |      |      |
| Esc+:w!      | To save forcefully                                 |      |      |
| Esc+wq!      | To save and quit forcefully                        |      |      |
| Esc+:x       | To save and quit                                   |      |      |
| Esc+:X       | To give passw or d to the file and remove password |      |      |
| Esc+:20(n)   | To go to line no 20 or n                           |      |      |
| Esc+: se nu  | To set the line numbers to the file                |      |      |
| Esc+:se nonu | To Remove the set line numbers                     |      |      |