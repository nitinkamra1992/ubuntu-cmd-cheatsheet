# VIM Basics

- VIM = Vi IMproved
- We will follow [this](https://www.youtube.com/watch?v=a6Q8Na575qc) lecture from MIT.
- Notation: `Control + V` is written in one of 3 ways:
  - `^V`
  - `ctrl-v`
  - `<C-V>`


## Files, windows and tabs

- Open a file with `vim {filename}`.
- VIM also has a model for windows, tabs and files (buffers).
- VIM maintains a set of open buffers, which means files for us.
- It can have tabs too, which can contain windows.
- But there need not be a 1:1 mapping b/w windows and buffers. 
- A window is just a view into a buffer (file).
- A buffer can be open in 0 or more windows at a time.
- By default, Vim opens with a single tab, which contains a single window.


## VIM modes

- VIM is a modal editor and has multiple operating modes, e.g., for reading, editing or writing code.
- Keys and key-combinations work differently in different modes.
- Vim is programmable (with Vimscript and also other languages like Python), and Vim’s interface itself is a programming language: keystrokes are commands, and these commands are composable.
- Vim avoids use of the mouse, because it’s too slow; Vim even avoids using the arrow keys because it requires too much movement. The end result is an editor that can match the speed at which you think.
- Type `vim` on your terminal and it starts up in normal mode, which allows normal file navigation.

#### Mode Switching

Use the following key combinations to enter another mode from Normal mode:
- `i`: Insert
- `shift-r` or `R`: Replace
- `v`: Visual Selection
- `shift-v`: Visual Selection Line
- `ctrl-v`: Visual Selection Block
- `:`: Command-line mode

Press `<ESC>` to go back to Normal mode from another mode, or to discard an unwanted incompletely typed command. If `<ESC>` is inconvenient to reach, you can bind `<CAPSLOCK>` for this purpose instead.

#### Normal mode - Navigation

- Navigate via the h, j, k and l keys (arrow keys also work, but hjkl are better reachable).
  - `h`: left
  - `j`: down
  - `k`: up
  - `l`: right
- `w` moves the cursor forward to the start of the next word, `e` moves it forward by a word to the end of the word, and `b` moves it backward by one word to the beginning of the word.
- `0` moves to the beginning of the line, `$` moves to the end of the line, `^` (carat) key moves to the first non-empty character of the line.
- `ctrl-u` takes you up and `ctrl-d` takes you down in the buffer.
- `G` moves you to the end of the buffer and `gg` moves you to the beginning.
- `ctrl-g` shows your current location in the file and file status.
- `:{number}` followed by `<ENTER>` or simply pressing`{number}G` navigates to line `{number}`.
- `L` moves you to the lowest line on the screen, `M` for middle of screen and `H` for highest line on the screen.
- `f{char}` finds the next occurence of the character `{char}` in that line, `F{char}` finds the previous occurence in that line. Use `;` and `,` to navigate matches.
- `t{char}` jumps to right before the next occurence of `{char}` in that line and `T{char}` jumps to right after the previous occurence of `{char}` in the line.
- `%` locates the next parentheses in the current line and jumps to the other matching parentheses if you are already at one.

#### Editing

- `i` enters Insert mode and allows editing text.
- `a` enters Append mode, which is same as Insert but positions the cursor 1 ahead of current position.
- `o` (`O`) opens a new line below (above) the current line, takes the cursor to the new line and starts Insert mode.
- `u` undoes the last command, `U` fixes a whole line, and `ctrl-r` is Redo.
- `x` deletes the current character (same as `dl`).
- `r{char}` replaces the current character with `{char}`.
- `d` is delete but is paired with a navigation command, e.g. `hjkl` or `wbe` etc. to delete characters/words etc. `d` actually cuts the text and stores it in a Vim register, so it can be pasted later. `dd` deletes the current line.
- `c` is change. Paired with navigation keys it deletes accordingly like `d`, but then puts you in insert mode to allow changing. `cc` deletes the line and puts in Insert mode.
- `s` opens Insert mode to substitute the current character (same as `cl`).
- `y` is copy (yank) and is paired with a navigation command to copy text. `yy` copies the current line.
- `p` pastes copied text right after the current character. If you delete a line with `dd` or yank it with `yy`, then `p` pastes it in the next line.
- `~` flips the case of the current character or the current selection.
- `.` repeats the last editing comand in VIM.

#### Selection

- Press `v` to enter visual mode.
- Using navigation commands in visual mode will select everything in between.
  - Selected text can be copied with `y` and pasted elsewhere if needed.
  - Selected text can be deleted with `d` or `x` in visual mode.
  - Selected text can be changed with `c` or `s` in visual mode.
  - Selected text can be written to file with `:w {filename}`. When you press `:` with text selected, you will see `:'<,'>` appear at the bottom of the screen. You should type `w {filename}` right after it to save the selected text to a file.
- `shift-v` is visual line mode and selects whole lines.
- `ctrl-v` is visual block mode and selects rectangular blocks.

#### Counts

You can combine commands with counts to repeat them. Examples:
- `4j` moves down 4 times.
- Press `v` to enter visual mode and then pressing `3e` selects till the end of the third word from the cursor.
- `7dw` deletes the next 7 words; `d7w` also deletes 7 words.

The general format is: `{command} [number] {motion}`

#### Modifiers

- Modifiers change the meaning of navigation commands a bit.
  - `i`: inside; `ci[` means change inside but not including the square brackets []. E.g., you can use it if you are positioned inside a markdown file on `n` in `[link]` in the text: `this is example text and [link](www.google.com)`. It'll delete `link` and put you in Insert mode. Similarly, `di[`.
  - `a`: "around" works similar to inside, except `da[` deletes both the square brackets and the contents in it.


## Commands

Command-line mode commands begin with `:` and are finished by pressing `<ENTER>`.

#### Help

- Use `:help {command}` or `:h {command}` to get help on another command, key or key combination. Use `:q` to close the help window. E.g., `:h w` or `:h CTRL-W`.

#### Files, windows and tabs

- `:q` or `:quit` terminates the current window. VIM will terminate if you terminate the last window.
- `:q!` forces termination even if you have unsaved changes to the open file.
- `:qa` is quit all windows.
- `:w` (write) command saves a file. You can also provide a filename to save to: `:w {filename}`.
- `:wq` performs a save and quit.
- `:e {filename}`: Open `{filename}` for editing.
- `:ls` shows open buffers.
- `:sp` (`:vsp`) splits open a new window horizontally (vertically) in the current tab.
- `:tabnew` opens a new tab.
- `gt` goes to the next tab (loops around from last to first tab).
- `ctrl-w` paired with `up`, `down`, `j` or `k` allows switching between windows.

#### Search and substitution

- `/{regex}` searches for the `{regex}` wrapping around the file if it hits EOF and takes the cursor to the first instance found. Pressing `n` (`N`) takes you to the next (previous) instance of `{regex}`. Adding `\c` like `/{regex}\c` allows you to ignore case for this particular search.
- `:s` is the substitute command ([docs](https://vim.fandom.com/wiki/Search_and_replace)).
  - `:s/foo/bar/g`: Find each occurrence of 'foo' (in the current line only) and replace it with 'bar'.
  - `:%s/foo/bar/g`: Find each occurrence of 'foo' (in all lines) and replace it with 'bar'.
  - `:%s/foo/bar/gc`: Change each 'foo' to 'bar', but ask for confirmation first.
  - `:%s/\<foo\>/bar/gc`: Change only whole words exactly matching 'foo' to 'bar'; ask for confirmation.
  - `:5,12s/foo/bar/g`: Change each 'foo' to 'bar' for all lines from line 5 to line 12 (inclusive).
  - `%s/\[.*\](\(.*\))/\1/g`: Replace named Markdown links with plain URLs.
  - The `g` flag means global – each occurrence in the line is changed, rather than just the first.
- `:/{regex}` also searches for all patterns matching the `{regex}` just like `/{regex}`. Can also append flags `/g` etc. like with substitute.

#### History

- `q:` shows your command history.
- `:` followed by `up` or `down` keys lets you search through your command history.

#### Other commands

- `:!{cmd}` lets you execute external commands on your terminal from vim. Examples:
  - `:!ls`
  - `:!mkdir {dirname}`
- `:r {filename}` retrieves the contents of `{filename}` and places them below the cursor line.
  - You can also read the output of an external command this way, e.g., `:r !ls` puts the output of `ls` command below the cursor line.
- Completion:
  - While typing commands, you can press `CTRL-D` to show possible completions.
  - Press `<TAB>` to complete with the first possible completion and repeat to cycle through the completions.

#### Setting options

- `:set {option}` allows you to set an option.
- In general, you can use either the long or the short option name.
- Prepend "no" to switch an option off.
- Examples:
  - `:set ic` sets `ignorecase` during search and substitution. To disable it `:set noic`.
  - `:set hls` or `:set hlsearch` sets `hlsearch` for highlighting search results. To disable, `:set nohls` or `:nohlsearch`.
  - `:set is` or `:set incsearch` sets incremental search to jump to the first result while you type the search command. Disable with `:set nois`.


## Configuring VIM

- Can be configured via a plain-text configuration file: `~/.vimrc` (containing Vimscript commands).
- See a starter `.vimrc` file [here](vimrc).


## Extending Vim

- There are tons of plugins for extending Vim.
- You do not need to use a plugin manager for Vim (since Vim 8.0). Instead, you can use the built-in package management system.
- Simply create the directory `~/.vim/pack/vendor/start/` and put plugins in there (e.g. via `git clone`).

Here are some good plugins:
- [ctrlp.vim](https://github.com/ctrlpvim/ctrlp.vim): fuzzy file finder
- [ack.vim](https://github.com/mileszs/ack.vim): code search
- [nerdtree](https://github.com/scrooloose/nerdtree): file explorer
- [vim-easymotion](https://github.com/easymotion/vim-easymotion): magic motions

Check out [Vim Awesome](https://vimawesome.com/) for more awesome Vim plugins.


## Vim-mode in other programs

Many tools support Vim emulation. The quality varies from good to great; depending on the tool, it may not support the fancier Vim features, but most cover the basics pretty well.

- For Vim bindings in shells:
  - For bash, use `set -o vi`.
  - For zsh, use `bindkey -v`.
- Additionally, you can set your terminal editor to be Vim with `export EDITOR=vim`. This is the environment variable used to decide which editor is launched when a program wants to start an editor. For example, git will use this editor for commit messages.


## Macros

Vim allows configuring macros (even recursive ones). See the Macros section [here](https://missing.csail.mit.edu/2020/editors/) for a brief tutorial.


## Resources

- `vimtutor` is a tutorial that comes installed with Vim.
- Type `:help user-manual` for more topics in Vim.
- [Vim Adventures](https://vim-adventures.com/) is a game to learn Vim.
- [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki).
- [Vim Advent Calendar](https://vimways.org/2019/) has various Vim tips.
- [Vim Golf](http://www.vimgolf.com/) is [code golf](https://en.wikipedia.org/wiki/Code_golf)" but where the programming language is Vim’s UI.
- [Vi/Vim Stack Exchange](https://vi.stackexchange.com/).
- [Vim Screencasts](http://vimcasts.org/).
- [Practical Vim](https://pragprog.com/titles/dnvim2/) (book).
