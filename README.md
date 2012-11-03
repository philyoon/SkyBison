SkyBison
========

Description
-----------

It is well established that some of Vim's cmdline commands can be a bit
awkward to use, especially in comparison to how keystroke-efficient most of
the rest of Vim's functionality is, and as a result there are a plethora of
plugins designed to make functionality such as :b and :tag more accessible.

SkyBison is another attempt at tackling this problem.  It differs from other
options in two key ways:
1. It is intended to have a clean, simple and small code base.
2. It is designed to support cmdline commands in general, and should work with
   just about any cmdline command (that cmdline-completion supports).
SkyBison achieves this by taking advantage of Vim's built-in support for
cmdline-completion rather than attempting to recreate the wheel.

More specifically, SkyBison helps the user select an argument for various
cmdline commands by comparing the user's input to acceptable arguments.  Once
the user has input enough information to uniquely identify one argument,
SkyBison will either immediately accept that argument or prompt the user to
press &lt;cr&gt; to select that argument.  For example:

If Vim has three (listed) buffers, ".vimrc", ".bashrc" and ".zshrc", and the
user calls SkyBison to find an argument for :b, the user will see the
following:

    [1] .vimrc
    [2] .bashrc
    [3] .zshrc
    :b 

If the user inputs "v", SkyBison will recognize that the user wants ".vimrc"
and select it (or prompt the user to hit &lt;cr&gt; to select it).  However, if the
user inputs "s" (which is in both .bashrc and .zshrc, but not in .vimrc), the
user will see the following:

    [1] .bashrc
    [2] .zshrc
    :b s

SkyBison recognizes that .vimrc is no longer an option and drops it from the
possibilities.  However, all of the remaining characters after "s" are shared
in the remaining options.  Here, the user could enter "1" or "2" to select an
option.  In fact, the user could have entered "1", "2" or "3" earlier to
select a buffer when all three were possibilities.

This works with any cmdline command which takes an argument and is supported
by cmdline-completion: not just :b, but also :tag (for jumping to a tag),
:e (for editing a file), :h (for help), amongst many others.

Setup
-----

SkyBison can be installed like most other Vim plugins.  On a Unixy system
without a plugin manager, the skybison.vim file should be located at:

    ~/.vim/plugin/skybison.vim

On a Unixy system with pathogen, the skybison.vim file should be located at:

    ~/.vim/bundle/skybison/plugin/skybison.vim

On a Windows system without a plugin manager, the skybison.vim file should be located at:

    %USERPROFILE%\vimfiles\plugin\skybison.vim

On a Windows system with pathogen, the skybison.vim file should be located at:

    %USERPROFILE%\vimfiles\bundle\skybison\plugin\skybison.vim

If you are using a plugin manager other than pathogen, see its documentation
for how to install skybison - it should be comparable to other plugins.

If you would like the documentation to also be installed, include skybison.txt
into the relevant directory described above, replacing "plugin" with "doc".

SkyBison does not include any mappings by default (as there are far to many
possibilities to expect to propose an option for all of them).  To create your
own, place a mapping commands in your .vimrc/_vimrc, such as these:

    nnoremap <leader>b :call SkyBison("b",1)<cr>
    nnoremap <leader>t :call SkyBison("tag",1)<cr>
    nnoremap <leader>e :call SkyBison("e",0)<cr>
    nnoremap <leader>h :call SkyBison("h",0)<cr>

With those, &lt;leader&gt;b will call SkyBison to find an argument for :b,
&lt;leader&gt;t will call SkyBison to find an argument for :tag, &lt;leader&gt;e will
call SkyBison to find an argument for :e, and &lt;leader&gt;h will call SkyBison
to find an argument for :h.  The second argument for the function determines
whether or not SkyBison will automatically select an argument when there is
only one possibility (1) or whether SkyBison should wait for the user to
explicitly press &lt;cr&gt; (0).  With auto-&lt;cr&gt; in use, SkyBison can make selecting
things extremely quickly.  For example, with the above mapping and the example
in skybison-intro, the user could select .zshrc with only three keystrokes:
"\bz".  If the user were to use a shorter mapping (e.g.: just &lt;space&gt; or
&lt;cr&gt;), it'd only be two keystrokes.

For commands which take a file path as an argument (such as :e), it is
recommended to set auto-&lt;cr&gt; to disabled (ie, second argument to the function
should be 0) so that SkyBison does not select a directory when you want to
select a file within that directory.  This is best understood through
experiementation - give both it SkyBison("e",0) and SkyBison("e",1) a try.

Usage
-----

Once a mapping (as described in skybison-setup) is called, the user will see
the what Vim considers as possible arguments.  From here, the user may:

- Press &lt;esc&gt; to abort, akin to c_&lt;esc&gt;
- Press ctrl-u to clear the thusfar entered text and start over, akin to
  c_ctrl-u
- Press ctrl-w to remove the word behind the cursor, akin to c_ctrl-w
- Press &lt;tab&gt; or ctrl-l to complete the shared part of the remaining possible
  arguments, akin to c_ctrl-l
- Press &lt;cr&gt; to select the entered argument as-is, even if it does not match
  any argument Vim recognizes as a possibility.  This is useful for saving or
  editing a new file.
- Press the number next to an option to select it
- Enter another character to narrow down the possible argument.  When there is
  only one possibility remaining, SkyBison will either select it or prompt the
  user to hit &lt;cr&gt; to select it, depending on preference set in
  skybison-setup.
