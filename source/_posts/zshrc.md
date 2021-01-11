---
title: zshrc
date: 2021-01-01 20:29:44
tags: [linux, shell]
cover: /img/zsh.png

---
```sh
## Environment: 18.04
## add this to fit my typing habits
alias ifconfig='ip -c a'
alias ipconfig='ip -c a'
alias ip="curl icanhazip.com" # Your public IP address
alias ssh='ssh -X'
alias apt-get='sudo apt-get'
alias cc='clear'

# update on one command ##
alias update='sudo apt-get update && sudo apt-get upgrade'

## add confirmation and verbose ##
alias mv='mv -vi'
alias rm='rm -vi'
alias cp='cp -vi'
alias ln='ln -vi'

# Syntax: "repeat [X] [command]"
function repeat()      
{
    local i max
    max=$1; shift;
    for ((i=1; i <= max ; i++)); do  # --> C-like syntax
        eval "$@";
    done
}

## for v4l2-ctl shortcuts ##
alias lscam='ls /dev/vi*'
setgain() { v4l2-ctl --device=/dev/video"$1" --set-ctrl=gain="$2"}
getgain() { v4l2-ctl --device=/dev/video"$1" --get-ctrl=gain}
setexp() { v4l2-ctl --device=/dev/video"$1" --set-ctrl=exposure_absolute="$2"}
getexp() { v4l2-ctl --device=/dev/video"$1" --get-ctrl=exposure_absolute}
getresfps() { v4l2-ctl --device=/dev/video"$1" --list-formats-ext}

## use serial debugger on putty
alias setputty='sudo chmod 666 /dev/ttyUSB0'

# fro tar
alias untar='tar -zxvf '

## for git
alias stat="git status -s"
alias undo="git checkout --"
```