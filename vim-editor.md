# A basic Vim configuration

I like to store my vimrc inside ~/.vim/vimrc

To easily find the location and name of the file on any operating system, run `:echo $MYVIMRC` in Vim.

If it's the first time setting up a .vimrc, going through arp242's wizard[^1] and/or going through "Learn Vimscript The Hard Way"[^2] is much better than just loading up someone's pre-written configuration. 

## Setting up the vimrc

A barebones vimrc[^5] looks like this:

```
filetype plugin indent on
set expandtab
set shiftwidth=4
set softtabstop=4
set tabstop=4
set number
set relativenumber
set smartindent
set showmatch
set backspace=indent,eol,start
syntax on
```

Mine ended up being way more complex so I will just link it [here]().

## Using Vim's help system

Questions? Vim has a help system. [`:help help-summary`](https://vimhelp.org/usr_02.txt.html#help-summary) gives you 
instructions on how to use it.

[`:help quickref`](https://vimhelp.org/quickref.txt.html) is a quick reference guide. 


[^1]: [arp242 - My first vimrc](https://www.arp242.net/my-first-vimrc)

[^2]: [Learn Vimscript the Hard Way](https://learnvimscriptthehardway.stevelosh.com/chapters/00.html)

[^3]: [VimConfig - Simple and sane Vim configuration](https://vimconfig.com/)

[^4]: [Doug Black - How to Vimrc](https://dougblack.io/words/a-good-vimrc.html)

[^5]: [Tony Btw - How to Customize Vim in 2026](https://www.tonybtw.com/tutorial/vim/)
