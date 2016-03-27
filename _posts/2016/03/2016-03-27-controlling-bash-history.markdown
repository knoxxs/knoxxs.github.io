---
layout: post
title:  Controlling bash history
date:   2016-03-27 10:11:00
categories: linux utility
tags: bash history persist linux
---

There are hell lot of things you can do with your terminal history. For some people its contains more than 50% of knowledge they have and I am one of them. So I decided to manage it in proper way.

## Code

```
#HISTORY
export HISTCONTROL=ignoredups:erasedups  # no duplicate entries
export HISTSIZE=100000                   # big big history
export HISTFILESIZE=1000000               # big big history
shopt -s histappend                      # append to history, don't overwrite it

# Save and reload the history after each command finishes
PROMPT_COMMAND="history -n; history -w; history -c; history -r; $PROMPT_COMMAND"
```


## References

1. [How To Use Bash History Commands and Expansions on a Linux VPS](https://www.digitalocean.com/community/tutorials/how-to-use-bash-history-commands-and-expansions-on-a-linux-vps) - A great article.
2. [The Definitive Guide to Bash Command Line History](http://www.catonmat.net/blog/the-definitive-guide-to-bash-command-line-history/)
3. [Bash History Cheat Sheet](http://www.catonmat.net/download/bash-history-cheat-sheet.pdf)
4. [Preserve bash history in multiple terminal windows](http://unix.stackexchange.com/questions/1288/preserve-bash-history-in-multiple-terminal-windows)
5. [HISTCONTROL](http://askubuntu.com/a/15929/78177)
6. [Making your BASH history more efficient - HISTCONTROL](http://jorge.fbarr.net/2011/03/24/making-your-bash-history-more-efficient/#HISTCONTROL)
7. [Bash history: “ignoredups” and “erasedups” setting conflict with common history across sessions](http://unix.stackexchange.com/questions/18212/bash-history-ignoredups-and-erasedups-setting-conflict-with-common-history)


