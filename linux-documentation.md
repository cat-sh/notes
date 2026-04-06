# Where to Find Linux Documentation

The man command calls a pager program to interface with *nix systems' manual pages. These pages used to be printed into a book called "Unix Programmer's Manual"!

For (most) commands associated with the GNU project, use `info`.

Verify command version info with `--version` flag and the dedicated `--help` for even more documentation[^6].

## Useful Options

- `man man`

Contains a helpful index that allows us to reference the different sections available. 

A shell pipe [^1] shows all the section pages available, including non listed ones:

`find /usr/share/man/man* -type f -exec basename {} \; | sed 's/.*\.\([^.]\+\)\.[^.].\+$/\1/g' | sort | uniq -c | sort -n -r`

- `man 1 intro`

A concise introduction into (perhaps) the most useful, basic commands used in most Unix systems.

- `man builtins`

Contains information on bash builtin commands. The command `type -a` can verify if a command is a shell builtin or not[^6].

- `whatis`

"Displays one-line manual page descriptions". If it returns the error message `nothing appropriate`, run `mandb`.

- `apropos`

Searches for keyword in manual pages descriptions. Similar to `man -k`.

## Change Default Pager for Readability

The default pager is less but can be changed via .bashrc [^5] to a program with syntax highlighting. For example:

```
export MANPAGER="vim +MANPAGER -"
export MANPAGER="most"
export MANPAGER="more"
export MANPAGER="sh -c 'col -bx | bat -l man -p'"
```

## Use GNOME Help

Opens GUI interface with support for hyperlinks.

``` 
yelp info:make
yelp man:ls
```

## Other Tools

- [cheat.sh](https://cheat.sh/)

Cheatsheet pages with no download required. Just `curl cht.sh/btrfs`

- [tldr](https://github.com/tldr-pages/tldr) 

Also available [online](https://tldr.inbrowser.app/)

- [https://github.com/filiparag/wikiman](https://github.com/filiparag/wikiman)

oOffline documentation search engine. It browses man pages by default but can be configured to use ArchWiki, Gentoo Wiki, FreeBSD docs and even TLDR Pages.


---

#### References

[^1]: [University of Helsinki - Computing Tools for CS Studies - Chapter 1](https://courses.mooc.fi/org/uh-cs/courses/computing-tools-for-cs-studies/chapter-1)

[^2]: [Learn Linux TV - Learn the "man" command](https://www.youtube.com/watch?v=7BG-Devm7sA)

[^3]: [RobertElderSoftware - STOP Using 'man' Pages Incorrectly!](https://www.youtube.com/watch?v=cnmtKv2kUXs)

[^4]: [Mental Outlaw - The Best Way to Learn Linux](https://www.youtube.com/watch?v=Dg2Lek-xN70)

[^5]: [Stack Overflow](https://unix.stackexchange.com/questions/758842/how-do-i-change-the-pager-variable-for-man-if-its-possible)

[^6]: [RobertElderSoftware - Linux Experts Read 'info' Pages](https://www.youtube.com/watch?v=vnBCnd2L0dY)
