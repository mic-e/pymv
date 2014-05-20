Simple renaming script, designed similar to the zsh zmv built-in.

For optimal user experience, disable globbing for calls to pymv:

`alias pymv='noglob pymv'` (zsh)

Example usage:

    mic@mic-nb /tmp/pymvdemo $ find
    .
    ./CD2
    ./CD2/03 track3.mp3
    ./CD2/02 track2.mp3
    ./CD2/01 track1.mp3
    ./CD1
    ./CD1/02 track2.mp3
    ./CD1/01 track1.mp3
    mic@mic-nb /tmp/pymvdemo $ pymv "CD*/%d *" "\1\2 \3"
    matcher:  ^CD([^/]*)/([0-9]+) ([^/]*)$
    replacer: \1\2 \3

    CD1/01 track1.mp3 > 101 track1.mp3
    CD1/02 track2.mp3 > 102 track2.mp3
    CD2/01 track1.mp3 > 201 track1.mp3
    CD2/02 track2.mp3 > 202 track2.mp3
    CD2/03 track3.mp3 > 203 track3.mp3

    5 files will be renamed.
    Do you want to continue [Y/n]? y
    rmdir CD2
    rmdir CD1
    mic@mic-nb /tmp/pymvdemo $ find
    .
    ./203 track3.mp3
    ./202 track2.mp3
    ./201 track1.mp3
    ./102 track2.mp3
    ./101 track1.mp3

    mic@mic-nb /tmp/pymvdemo $ pymv "?%d *" "CD*/* *"
    matcher:  ^([^/])([0-9]+) ([^/]*)$
    replacer: CD\g<1>/\g<2> \g<3>

    101 track1.mp3 > CD1/01 track1.mp3
    102 track2.mp3 > CD1/02 track2.mp3
    201 track1.mp3 > CD2/01 track1.mp3
    202 track2.mp3 > CD2/02 track2.mp3
    203 track3.mp3 > CD2/03 track3.mp3

    5 files will be renamed.
    Do you want to continue [Y/n]? y
    mkdir CD1
    mkdir CD2
    mic@mic-nb /tmp/pymvdemo $ find
    .
    ./CD2
    ./CD2/03 track3.mp3
    ./CD2/02 track2.mp3
    ./CD2/01 track1.mp3
    ./CD1
    ./CD1/02 track2.mp3
    ./CD1/01 track1.mp3

To directly enter the matcher regular expression, use `--advanced`:

    pymv -a '^CD(.)/([0-9]+) (.*)$' '\1\2 \3'

To enter python lambdas for the matcher/replacer, use `--functor`.

    pymv -f 'f.startswith("CD")' '"CD #" + f[2:]'

Use `-i` to ignore case, `--depth` to limit the serach depth, and `--preserve-emptied` to prevent deletion of emptied directories.

Use `--help`, or the source code, for details.

Your feedback is always welcome on irc.freenode.net/#sfttech, as are your pull requests.
