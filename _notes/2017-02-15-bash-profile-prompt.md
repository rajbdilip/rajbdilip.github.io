---
title: "Bash prompt with PWD + Git branch on macOS"
date: 2017-02-15
---

Add this to your `~/.bash_profile` to show the current directory and Git branch in your terminal prompt.

```bash
# Add the following lines of code to ~/.bash_profile

parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "

# Run the following code and restart terminal
# $ bash ~/.bash_profile
```
