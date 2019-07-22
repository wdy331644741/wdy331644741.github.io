---
title: bashrc中添加git
date: 2018-04-27 23:00:34
categories:
- linux
tags:
- git
#top: true
---


```
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias la='ls -a'

# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi


export NVM_DIR="/root/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm


#function
#mkdir and enter it
mkcd(){
	mkdir "$1" && cd "$1"
}
#alias for cnpm
alias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"

#source ~/git/nvm.sh
source ~/.git-completion.bash
# 文件：https://github.com/git/git/tree/master/contrib/completion
# 显示分支官方实现
GIT_PS1_SHOWDIRTYSTATE=true
GIT_PS1_SHOWCOLORHINTS=true
if [ -f ~/.git-completion.bash ]; then
  source ~/.git-prompt.sh
  PROMPT_COMMAND='__git_ps1 "[\t][\u@\h:\w]" "\\\$ "'
fi

```
