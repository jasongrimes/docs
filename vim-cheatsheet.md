Indentation
===========

* Re-indent select text (in visual mode): `=`
* Indent current line one tab width: `>>`
* Indent selected text one tab width: `>`
* Indent selected text 2 tab widths: `2>`
* Toggling auto-indenting for code paste: `<F2>` # (See [this vim tip](http://vim.wikia.com/wiki/VimTip906))
* Change the indent width to 2: `:set sw=2 ts=2`
* Insert tabs as spaces: `:set expandtab`
* Convert all existing tabs to spaces: `:retab`

Selecting text
==============

* Visual select entire lines: `V`
* Select all: `ggVG`
* Re-select the last selected visual area: `gv`

Copy / paste
============

* Paste from a register without leaving insert mode: `<C-r>{register}` (ex: `<C-r>0`) 
* Copy an entire line: `yy`
* Duplicate a line, below: `yyp`
* Duplicate a line, above: `yyP`

Search and replace
==================

Search:

* Search: `/{text}` (forward) or `?{text}` (backward)
* Repeat last search: `n` (forward) or `N` (backward)
* Case-insensitive search: `/\c{text}` or `/{text}\c`
* Search for the word under the cursor: `.` (forward) or `#` (backward)

Replace:

* Search and replace in entire file: `:%s/foo/bar/g`
* Search and replace in entire file, with confirmation: `:%s/foo/bar/gc`
* Case insensitive search and replace in entire file, with confirmation: `:%s/foo/bar/gci`
* Whole word search and replace with confirmation: `:%s/\<foo\>/bar/gc`
* Search and replace in selection: `:'<,'>s/foo/bar/` (The range (`'<,'>`) is entered automatically if there's a visual highlight.)

Navigating within a file
========================

* Go back to previous cursor position: `CTL-o`
* Go forward to next cursor position (after going back): `CTL-i`
* Go to last change: `'.`
* Go to last position before search/jump: `''`

Navigating source code with ctags
=================================

* Regenerate the tags file from within vim: `:!ctags -R`
* Jump to definition of keyword under cursor: `<C-]>`
* Select from list of matching keywords under cursor (if multiple): `g<C-]>`
* Jump to named keyword definition: `:ta <tag>` or `:tag <keyword>`
* If multiple definitions were matched, use these to navigation through the rest:
    - `:ts` list matching tags
    - `:tn` next tag in the list
    - `:tp` previous tag in the list
    - `:tf` first tag in the list
    - `:tl` last tag in the list
* Jump to or list multiple named keyword definitions: `:tjump <keyword>`
* Go back in tag history: `<C-t>`

Toggle tag list (file/class structure) in a side window: `:Tlist` (requires taglist plugin)
Then move to the tag window (`<C-w> h`) and press return on the tag to navigate to.

Files and buffers
==================

* List buffers: `:ls`
* Switch to next file in buffer: `:bn`
* Switch to previous file in buffer: `:bp`
* Switch to numbered file in buffer: `:b<num>`
* Switch to buffer name: `:b<name>`
* Toggle between last buffer: `CTL-^`
* Delete a buffer: `:bd`

Windows
=======

A window is a viewport on a buffer.

* Moving to another window: `<C-w>` and then a direction, h for left, l for right, j for down, k for up. Ex. `<C-w> h` or `<C-w> <C-h>`
* Quit the current window: `:q` or `<C-w> q`
* Close the current window: `:clo` or `<C-w> c` (seems to be like quit, but fails if it's the last open window)
* Close all windows other than the current one: `:on` or `C-w C-o`
* Split current window into two viewports (horizontal): `:sp` or `<C-w> s`
* Split current window into two viewports (vertical): `:vs` or `<C-w> v`
* Create new window (horizontally) and start editing empty file in it: `:new`
* Create new window (vertically) and start editing empty file in it: `:vne` 

Tabs
====

A tab is a collection of windows.

* Open a new tab: `:tabe <file>` " Or use a directory instead of a file (ex. ".") and you can browse to the file.
* To close a tab, use `:wq`, etc.
* Switch to next tab: `:tabn`
* Switch to previous tab: `:tabp`

* Open a new tab for each open buffer: `:tab sball`

Tips on making tabs behave like other editors: http://stackoverflow.com/a/3476411/1124393

Navigating files with find
==========================

First set the path in which to search: (`**` matches all subdirectories) 

    :set path+=**

...or, if you don't want the path rooted in the directory in which you launched vim:

    :set path+=~/dev/playgrounds/** 

Look up files by name (within the path): `:find <file>`

* Go to file whose name is under the cursor: `gf`
  This only works to jump to the first matching file. (Can also prefix with a number to specify the nth matching file.)
  May want to force the path before using this command if the project has many files with the same name, ex. `:path=src/Grit/RpgBundle/**`

* Go back to an older position in the jump list (ex. previous file, previous position in file): `<C-o>`
* Go forward, to next position in the jump list: `<C-i>`

File explorer (NERDTree)
========================

* Open the file explorer in a vertical split window: `:NERDTree` (or via my shortcut in .vimrc: `:NE`)
* Open the file explorer and jump to the file in the current buffer: `NERDTreeFind` (or my shortcut, `:NEF`)
* Add a bookmark: highlight the tree item and then `:Bookmark <name>` (name is optional)
* Toggle display of bookmarks: `B`
* Delete selected bookmark: `D`
* Change tree root to the selected dir: `C`
* Toggle help: `?`

File explorer (netrw)
=====================

netrw is vim's "native" file explorer--it's a plugin that is usually distributed with vim. (NERD Tree is another
popular plugin but it replaces the functionality of netrw.) 

* Open file explorer in project root directory: `:e.` or `:edit .`
* Open file explorer in dir of last file opened in current window: `:E` or `:Explore`
* Split window horizontal `:e.` (root dir): `:sp.` or `:split .`
* Split window vertial `:e.` (root dir): `:vs.` or `:vsplit .`
* Split window horizontal `:E` (cwd): `:Sex`
* Split window vertical `:E` (cwd): `:Vex`

Close split file explorer windows with `:bd` or `:q`.

Working within the file explorer:

* Get help: `<F1>`
* Move up a directory: `-`
* Sort: `s`
* Rename: `R`
* Delete: `D`
* New file: `%`
* New directory: `d`

To see the file explorer in a tree view, instead of the default flat view, add the following to .vimrc:

    let g:netrw_liststyle=3

File formats
============

See http://vim.wikia.com/wiki/File_format

* See the fileformat used in the current buffer: `:set ff?`
* See the global file format options in use: `:set ffs?`
* Set the format of the current buffer to use unix line endings: `:set ff=unix`

Folding
=======

* Open one level of folds within the whole buffer ("reduce" folds): `zr`
* Open all folds in the whole buffer: `zR` 
* Close one level of folds within the whole buffer ("more" folds): `zm`
* Close all folds: `zM`
* Open one level of fold under cursor: `zo`
* Close one level of fold under cursor: `zc`
* Toggle one level of fold under cursor: `za`

Word wrap
=========

* Set the text width (wraps as close to 80 chars as whitespace allows without exceeding it): `:set tw=80`
* Disable text width: `:set tw=0`

To wrap text, either visually select a block of text and type `gq`, 
or else move the cursor to the beginning of the line to format, 
type `gq`, and then specify the range, ex. `$` to format to the end of the line.

Misc
====

* Go to line 23: `:23` 
* Duplicate current line: `yyp` or `:t.`
* Enable or disable line numbers: `set number` and `set nonumber`

Color schemes
=============

See http://vimcasts.org/episodes/creating-colorschemes-for-vim/

* Changing color scheme: `:colorscheme <name>`
* Determine the highlighting group of the current word: `<C-Shift-P>` (requires appropriate code snippet in .vimrc)

Once you know the highlighting group, change it in your colorscheme file (stored in .vim/colors/<name>.vim) like this:

    highlight <group> ctermfg=#<hexcolor> ctermbg=NONE cterm=NONE

"ctermfg" means foreground color, "ctermbg" means background color, and cterm can be "bold", etc.

Invisible characters
====================

* Enable display of "invisible" characters (tabs and newlines): `:set list`
* Disable display of "invisible" characters: `:set nolist`
* Toggle display of invisible characters: `:set list!`

Links
=====

* Symfony: http://knplabs.com/blog/boost-your-productivity-with-sf2-and-vim
* PHPDoc: http://www.vim.org/scripts/script.php?script_id=1355

