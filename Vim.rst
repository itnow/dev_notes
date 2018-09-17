###############################################################################
 Vim notes
###############################################################################

.. code-block:: vim

    :help runtimepath
    :echo $VIMRUNTIME



Compile
===============================================================================

.. code-block:: bash

    $ cd /usr/local/src
    # Clone or download from https://github.com/vim/vim/releases
    $ sudo git clone https://github.com/vim/vim.git
    $ cd vim/src
    $ make distclean  # if you build Vim before

    $ sudo ./configure \
        --enable-pythoninterp=dynamic \
        --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu \
        --enable-python3interp=dynamic \
        --with-python3-config-dir=/usr/lib/python3.6/config-3.6m-x86_64-linux-gnu

    $ sudo make
    $ sudo make install



Motion
===============================================================================

=============== ===============================================================
w               Move to next word.
W               Move to next blank delimited word.
b               Move to the beginning of the word.
B               Move to the beginning of blank delimted word.
e               Move to the end of the word.
E               Move to the end of Blank delimited word.
0               Move to the begining of the line.
$               Move to the end of the line.
gg / 1G / :0    Move to the beginning of the file.
G / :$          Move to the end of the file.
ngg / nG / :n   Move to n-th line of the file.
fc              Move forward to c.
Fc              Move back to c.
Ctrl + d        Go down half a screen.
Ctrl + u        Go up half a screen.
Ctrl + f        Go forward a screen.
Ctrl + b        Go back a screen.
H               Move to top of screen.
M               Move to middle of screen.
L               Move to botton of screen.
(               Jumps to the previous sentence.
)               Jumps to the next sentence.
{               Jumps to the previous paragraph.
}               Jumps to the next paragraph.
[[              Jumps to the previous section.
]]              Jumps to the next section.
[]              Jump to the end of the previous section.
][              Jump to the end of the next section.
=============== ===============================================================



Insert text
===============================================================================

=== ===========================================================================
i   Insert before cursor.
I   Insert before line.
a   Append after cursor.
A   Append after line.
o   Open a new line after current line.
O   Open a new line before current line.
r   Replace one character.
R   Replace many characters.
=== ===========================================================================



Specials inserts
===============================================================================

=============== ===============================================================
:r [filename]   Insert the file [filename] below the cursor
:r ![command]   Execute [command] and insert its output below the cursor
=============== ===============================================================



Change text
===============================================================================

- In visual mode select the text: ``U`` for uppercase, ``u`` for lowercase, ``~``
  to swap all casing.

- VISUAL MODE -> select lines -> ``gq`` - wrap to rulers.

- ``:retab [new_tabstop]`` - Replace all sequences of white-space containing
  a <Tab> with new strings of white-space using the "new_tabstop" value given
  or the current value of "tabstop" (``:help retab``). ``:retab`` accepts
  a range and works with selected lines too.

=========== =======================================================================
C           Change to the end of the line
cc          Change the whole line
guu         Lowercase line
gUU         Uppercase line
~           Switch upper/lower case under cursor or selection
g~~         Invert case to entire line
gu<motion>  Make the characters in motion lowercase.
gU<motion>  Make the characters in motion uppercase
=========== =======================================================================



Deleting (cut) text
===============================================================================

When performing copy, cut, and paste by default Vim uses its own location
for this, called the unnamed register.

=========== ===================================================================
x           Delete character under cursor
X           Delete character to the left of cursor
D           Delete (cut) to the end of the line
dd / :d     Delete (cut) current line
ndd         Delete (cut) n-th lines
dw          Delete (cut) a word
d0          Delete (cut) to the beginning of a line
d$          Delete (cut) to the end of a line
=========== ===================================================================



Yanking & pasting
===============================================================================

http://vim.wikia.com/wiki/Accessing_the_system_clipboard

=========== ===================================================================
yy / :y     Copy current line into storage buffer
p           Paste storage buffer after current line
P           Paste storage buffer before current line
"xyy        Copy the current lines into register x
"xp         Paste from register x after current line
yyp         Duplicate line
=========== ===================================================================



Undo/Redo
===============================================================================

=========== ===================================================================
u           Undo the last operation.
Ctrl + r    Redo the last undo.
=========== ===================================================================



Search & replace
===============================================================================

Searching for the current word: in normal mode, move the cursor to any word and
press ``*`` to search forwards for the next occurrence of that word, or press
``#`` to search backwards. It searches for the exact word at the cursor,
searching for "rain" would not find "rainbow". Use ``g*`` or ``g#`` if you
don't want to search for the exact word.

================= =============================================================
/keyword          Search forward
?keyword          Search backward
n                 Search for next instance of string
N                 Search for previous instance of string
ggn               Jump to the first match
GN                Jump to the last
:%s/orig/repl     Search for the first occurrence of the string “original” and
                  replace it with “replacement”.
:%s/orig/repl/g   Search and replace all occurrences of the string “original”
                  with “replacement”.
:%s/orig/repl/gc  Search for all occurrences of the string “original” but ask
                  for confirmation before replacing them with “replacement”.
================= =============================================================



Bookmarks
===============================================================================

=============== ===============================================================
m {a-z A-Z}     Set bookmark {a-z A-Z} at the current cursor position
:marks          List all bookmarks
\`{a-z A-Z}     Jumps to the bookmark {a-z A-Z}
=============== ===============================================================



Select & modify text
===============================================================================

=== ===========================================================================
v   Enter visual mode per character
V   Enter visual mode per line
~   Switch case
d   Delete (cut)
c   Change
y   Yank
>   Shift right
<   Shift left
=== ===========================================================================



Help navigation
===============================================================================

========= =====================================================================
Ctrl+]    Jump to the definition of the keyword under the cursor.
Ctrl+t    Go back.
========= =====================================================================



Insert Tab
===============================================================================

In insert mode <CTRL-V> inserts a literal copy of your next character.
To input tab instead of expanded spaces press <CTRL-V><Tab>.



File operation
===============================================================================

=========================== ===================================================
:Ex                         Open Explorer
:file :f                    Prints the current file name
:saveas :sav {file}         Save the current buffer under the name {file} and
                            set the filename of the current buffer to {file}.
                            The previous name is used for the alternate file
                            name.
:view                       Switch to RO current file.
:view {path/to/file}        Open file for view.
:e[dit]                     Edit current file or to force refresh current file.
:e[dit] [path/to/file]      Open file for edit.
:find {file}                Find file in 'path' and then :edit it.
:cd [go/to/path]
:pwd                        Print the current dir name
=========================== ===================================================



Buffers
===============================================================================

:ene :enew
    Edit a new, unnamed buffer.

:ene! :enew!
    Discard any changes for current buffer and edit a new buffer.

:files :buffers :ls
    Show list of all buffers.

:bd :bdel :bdelete
    Unload buffer (if not changed) and delete from buffer list.

:bd[!] :bdel :bdelete
    Drop changes and unload buffer.



Windows split
===============================================================================

:split | :sp | CTRL-W s
    Split horizontal.

:vsplit | :vs | CTRL-W v
    Split vertical.

:quit | :q
    Quit current window. Quit vim if last window.

:only | :on
    Make current window the only one on the screen.



Window resizing
===============================================================================
``:help window-resize``

Make windows equals width:

- :wincmd =
- :winc =
- CTRL-W =

Add more/less n chars wide for horizontal/vertical split:

- :resize [n]
- :res [n]
- :vertical resize [n]
- :vert res [n]
- :[n]winc >
- :[n]winc <
- [n]CTRL-W >
- [n]CTRL-W <
- CTRL-W [n] >
- CTRL-W [n] <

For horizonal split use ``-``/``+`` instead of ``<``/``>``:

- :[n]winc -
- :[n]winc +
- [n]CTRL-W -
- [n]CTRL-W +



Navigate between windows
===============================================================================

``CTRL-W j`` | ``CTRL-W <Down>`` | ``CTRL-W CTRL-J``
    Move cursor to Nth window below current one. Uses the cursor position to
    select between alternatives.

``CTRL-W k`` | ``CTRL-W <Up>`` | ``CTRL-W CTRL-K``
    Move cursor to Nth window above current one. Uses the cursor position to
    select between alternatives.

``CTRL-W h`` | ``CTRL-W <Left>`` | ``CTRL-W CTRL-H`` | ``CTRL-W <BS>``
    Move cursor to Nth window left of current one. Uses the cursor position to
    select between alternatives.

``CTRL-W l`` | ``CTRL-W <Right>`` | ``CTRL-W CTRL-L``
    Move cursor to Nth window right of current one. Uses the cursor position
    to select between alternatives.

``CTRL-W w`` | ``CTRL-W CTRL-W``
    Without count: move cursor to window below/right of the current one. If
    there is no window below or right, go to top-left window. With count: go to
    Nth window (windows are numbered from top-left to bottom-right). When N is
    larger than the number of windows go to the last window.

``CTRL-W W``
    Without count: move cursor to window above/left of current one. If there
    is no window above or left, go to bottom-right window. With count: go to
    Nth window, like with CTRL-W w.



Windows Moving
===============================================================================

``CTRL-W r`` | ``CTRL-W CTRL-R``
    Rotate windows downwards/rightwards. The first window becomes the second
    one, the second one becomes the third one, etc. The last window becomes
    the first window. The cursor remains in the same window.

    This only works within the row or column of windows that the current window
    is in.

``CTRL-W R``
    Rotate windows upwards/leftwards. The second window becomes the first one,
    the third one becomes the second one, etc. The first window becomes the
    last window. The cursor remains in the same window.

    This only works within the row or column of windows that the current window
    is in.

``CTRL-W x`` | ``CTRL-W CTRL-X``
    Without count: Exchange current window with next one. If there is no next
    window, exchange with previous window.  With count: Exchange current window
    with Nth window (first window is 1). The cursor is put in the other window.

    When vertical and horizontal window splits are mixed, the exchange is only
    done in the row or column of windows that the current window is in.

The following commands can be used to change the window layout. For example,
when there are two vertically split windows, ``CTRL-W K`` will change that in
horizontally split windows. ``CTRL-W H`` does it the other way around.

``CTRL-W K``
    Move the current window to be at the very top, using the full width of the
    screen. This works like closing the current window and then creating
    another one with ``:topleft split``, except that the current window
    contents is used for the new window.

``CTRL-W J``
    Move the current window to be at the very bottom, using the full width of
    the screen. This works like closing the current window and then creating
    another one with ``:botright split``, except that the current window
    contents is used for the new window.

``CTRL-W H``
    Move the current window to be at the far left, using the full height of the
    screen. This works like closing the current window and then creating
    another one with ``:vert topleft split``, except that the current window
    contents is used for the new window.

    Not available when compiled without the ``|+vertsplit|`` feature.

``CTRL-W L``
    Move the current window to be at the far right, using the full height of
    the screen. This works like closing the current window and then creating
    another one with ``:vert botright split``, except that the current window
    contents is used for the new window.

    Not available when compiled without the ``|+vertsplit|`` feature.



Terminal
===============================================================================

``:help terminal``

Run a terminal emulator in a vim window. To run a shell::

    :term bash

Or to run build command::

    :term make myprogram

Use ``CTRL-W w`` to navigate between windows. See ``:help terminal-typing``.



Shell
===============================================================================

=================== ===========================================================
:!{command}         Run a shell command, shows the output and prompts before
                    returning to the current buffer.
:!                  Runs the last external command (from shell history).
:!!                 Repeats the last command.
:silent !{command}  Eliminates the need to hit enter after the command is done.
:r !{command}       Puts the output of command into the current buffer.
Ctrl+z              Will suspend the Vim process and get back to your shell.
fg                  Will resume (bring to foreground) your suspended Vim.
:sh                 Start a subshell.
:set shell?         Show configured shell.
Ctrl+d / ``exit``   To kill the shell and return to vim.
=================== ===========================================================



vimdiff
===============================================================================

Run vimdiff:

.. code-block:: bash

    $ vim -d <file1> <file2> [file3 [file4]]
    $ vimdiff <file1> <file2> [file3 [file4]]

:diffthis
    to initiate a diff when Vim is already running

:diffoff
    to turn diff off

:dif :diffupdate
    Force the differences to be updated. Vim attempts to keep the differences
    updated when you make changes to the text. This mostly takes care of
    inserted and deleted lines. Changes within a line and more complicated
    changes do not cause the differences to be updated.

:dif! :diffupdate!
    If the ``!`` is included Vim will check if the file was changed externally
    and needs to be reloaded. It will prompt for each changed file, like
    ``:checktime`` was used.

:windo diffthis :windo diffoff
    Diff on/off two split windows

=============== ===============================================================
dp / diffput    Puts changes under the cursor into the other file
                making them identical (thus removing the diff).
do / diffget    The change under the cursor is replaced by the content
                of the other file making them identical (o => obtain).
]c              Jump to the next diff.
[c              Jump to the previous diff.
zo              Open folded text.
zc              Close folded text.
zi              Enable/disable folding.
=============== ===============================================================

- ``:help fold-commands``
- http://vimdoc.sourceforge.net/htmldoc/usr_28.html



Plugins
===============================================================================

vim-plug
--------
https://github.com/junegunn/vim-plug

Automatic installation and config example:

.. code-block:: vim

    " Load vim-plug
    "
    if empty(glob('~/.vim/autoload/plug.vim'))
        silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
            \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
        autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
    endif
    " --sync flag is used to block the execution until the installer finishes.

    call plug#begin('~/.vim/plugged')
    " Make sure you use single quotes

    " Shorthand notation; fetches https://github.com/vim-airline/vim-airline
    Plug 'vim-airline/vim-airline'

    " Any valid git URL is allowed
    Plug 'https://github.com/junegunn/vim-github-dashboard.git'

    " On-demand loading
    Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle' }
    Plug 'tpope/vim-fireplace', { 'for': 'clojure' }

    " Unmanaged plugin (manually installed and updated)
    Plug '~/my-prototype-plugin'

    " Initialize plugin system
    call plug#end()

    " Reload .vimrc and :PlugInstall to install plugins.

If you're behind an HTTP proxy, you may need to add ``--insecure`` option to
the curl command. In that case, you also need to set ``$GIT_SSL_NO_VERIFY`` to
true.



vim-airline
-----------
https://github.com/vim-airline/vim-airline

.. code-block:: vim

    " Airline
    " -------
    " The following are some unicode symbols for customizing the left/right
    " separators, as well as the powerline font glyphs. We must define
    " the dictionary first before setting values. Check whether it exists
    " as to avoid accidentally overwriting its contents.
    if !exists('g:airline_symbols')
        let g:airline_symbols = {}
    endif
    " Unicode symbols
    let g:airline_left_sep = '»'
    let g:airline_left_sep = '▶'
    let g:airline_right_sep = '«'
    let g:airline_right_sep = '◀'
    let g:airline_symbols.crypt = '🔒'
    let g:airline_symbols.linenr = '␊'
    let g:airline_symbols.linenr = '␤'
    " let g:airline_symbols.linenr = '¶'
    " let g:airline_symbols.maxlinenr = '☰'
    let g:airline_symbols.maxlinenr = ''
    let g:airline_symbols.branch = '⎇'
    let g:airline_symbols.paste = 'ρ'
    " let g:airline_symbols.paste = 'Þ'
    " let g:airline_symbols.paste = '∥'
    let g:airline_symbols.spell = 'Ꞩ'
    let g:airline_symbols.notexists = '∄'
    let g:airline_symbols.whitespace = 'Ξ'

    " Enable the list of buffers
    let g:airline#extensions#tabline#enabled = 1
    " Show just the filename
    let g:airline#extensions#tabline#fnamemod = ':t'


NERDTree
--------
https://github.com/scrooloose/nerdtree

.. code-block:: vim

    " NERDTree
    " --------
    let NERDTreeShowHidden = 1        " show hidden files
    map <F2> :NERDTreeToggle<CR>



commentary.vim
--------------
https://github.com/tpope/vim-commentary

Use ``gcc`` to comment out a line (takes a count), ``gc`` to comment out the
target of a motion (for example, ``gcap`` to comment out a paragraph), ``gc``
in visual mode to comment out the selection, and ``gc`` in operator pending
mode to target a comment.



Supertab
--------
https://github.com/ervandew/supertab



fugitive.vim
------------
https://github.com/tpope/vim-fugitive


:Gstatus
    Bring up the output of git-status in the preview window.
    Run ``:h Gstatus`` for more info.

:Gdiff [revision]
    Perform a vimdiff against the current file in the given revision.



vim-gitgutter
-------------
https://github.com/airblade/vim-gitgutter

======= ===================================================
]c      Jump to next hunk (change)
[c      Jump to previous hunk (change)
======= ===================================================



ctrlp.vim
---------
https://github.com/ctrlpvim/ctrlp.vim

.. code-block:: vim

    " CtrlP
    "
    nnoremap <leader>p :CtrlP<cr>

    " Easy bindings for its various modes
    nnoremap <leader>bb :CtrlPBuffer<cr>
    nnoremap <leader>bm :CtrlPMixed<cr>
    nnoremap <leader>bs :CtrlPMRU<cr>

    let g:ctrlp_switch_buffer = 0
    let g:ctrlp_working_path_mode = 0

    " Show dot files
    let g:ctrlp_show_hidden = 1

    " Setup some default ignores
    let g:ctrlp_custom_ignore = {
        \ 'dir':  '\v[\/](\.(git|hg|svn)|\_site)$',
        \ 'file': '\v\.(exe|so|dll|class|png|jpg|jpeg)$',
    \}



jedi-vim
--------
https://github.com/davidhalter/jedi-vim

.. code-block:: vim

    " Jedi-vim
    "
    " Disable docstring preview window
    autocmd FileType python setlocal completeopt-=preview
    " alternative variant:
    "   let g:jedi#auto_vim_configuration = 0
    "   set completeopt=menuone,longest



Some other plugins
------------------

- https://github.com/Yggdroot/indentLine
  (`json syntax conflicts <https://github.com/Yggdroot/indentLine/issues/140>`_)
- https://github.com/nathanaelkane/vim-indent-guides
- https://github.com/tpope/vim-surround
- https://github.com/vim-syntastic/syntastic
- https://github.com/easymotion/vim-easymotion



Schemas
===============================================================================

- https://github.com/morhetz/gruvbox
- https://github.com/nanotech/jellybeans.vim
- https://github.com/w0ng/vim-hybrid
- https://github.com/sickill/vim-monokai
- https://github.com/tomasr/molokai

