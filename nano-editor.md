# nano - text editor inspired by Pico

- Open nano on a specific line 

When opening a file via command line:

`nano +c/Foo file`

It can be put on the first or last occurence of that string by specifying after +/

String can be case sensitive and regex can be used by inserting c and/or r after
the + sign 

- `nano -`

Allows nano to read data from standard input


## Shortcuts

^K - cut text
^6 - copy / mark text
Alt + # - show line numbers (can also be set in nanorc with `set linenumbers`) 

