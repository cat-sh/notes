# `grep` - search globally for a regular expression and print

The command `grep` allows you to filter out specific lines from a file.

## Exercise

Instructions provided [here](https://courses.mooc.fi/org/uh-cs/courses/computing-tools-for-cs-studies/chapter-1)

> By default grep is case sensitive, which means that it treats "a" and "A" differently when searching for matches. Take a look at the output of `grep --help` or grep's man page, and find out how you can make grep ignore case.

> Find out (using Google for example) how you can only match occurences which are at the beginning of a line. Make sure you understood how to do this, by practicing using the command on the command line. You can for example write the following to a file:

```
Unix
Linux
macOS
UNiX
unique-unix
unIX
Operating system unix
UNIX
unix
unisport
```

> and then make sure you can filter out the appropriate words with grep.

## Solutions

### Ignore case

`grep -i "unix" file.txt`

### Match only at the beginning of a line

`grep -i "^unix" file.txt` 

### Match only exact word "unix" at start

`grep -i "^unix\b" file.txt`

