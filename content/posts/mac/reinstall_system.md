---
title: "Reinstall System"
date: 2019-06-06T18:40:25Z
draft: false
categories: [Mac]
tags: []
card: false
weight: 0
---

What I have to do after reinstalling the system

<!--more-->

## Install Brew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

**关闭每次运行时的自动更新**

```bash
	# 临时关闭  在Terminal中运行
	# 永久关闭  在Terminal配置文件中加入下面这一行 .bash_profile
export HOMEBREW_NO_AUTO_UPDATE=true
```

## 安装 g++

```bash
# 查询可用版本
brew search gcc
# 安装
brew install gcc@7
```

**设置gcc7为默认编译器**

```bash
# Setting gcc7 as default gcc compiler
alias gcc='gcc-7'
alias gcc='g++-7'
alias gcc='cpp-7'
alias gcc='c++-7'
```

## .bash_profile

```bash
# System Command
alias ll='ls -al'
alias c='clear'

# Setting for my command
export PATH=$PATH:~/.my_command

# Setting gcc7 as default gcc compiler
alias gcc='gcc-7'
alias gcc='g++-7'
alias gcc='cpp-7'
alias gcc='c++-7'

# For ACM-ICPC
alias acm='cd /Users/akvicor/Documents/GitHub/Problem-set'

# Use 'safe-rm' as 'rm'
alias rm='safe-rm'

# Close Homebrew auto update
export HOMEBREW_NO_AUTO_UPDATE=true
```

## .vimrc

```bash
filetype plugin indent on
colorscheme desert
syntax on
set nu
set backspace=2
set hlsearch 
set syntax=on
set tabstop=2
set shiftwidth=2
set smarttab
set smartindent
set showmatch
set matchtime=0
set report=0

function Close(char)
    if getline('.')[col('.') - 1] == a:char
        return "\<Right>"
    else
        return a:char
    endif
endfunction

map <C-A> ggVG"+y
map <F5> :call Run()<CR>
map <F4> :call Run2()<CR>
func! Run()
    exec "w"
    exec "!g++-7 -O2 -std=c++11 -Wall % -o .%<.sol"
    exec "!./.%<.sol"
endfunc
func! Run2()
    exec "w"
    exec "!g++-7 -O2 -std=c++11 -Wall % -o .%<.sol"
    exec "!./.%<.sol < in"
endfunc

map <F12> :call SetTitle()<CR>
func SetTitle()
let l = 0
let l = l + 1 | call setline(l,'/* ***********************************************')
let l = l + 1 | call setline(l,'Author        : Akvicor')
let l = l + 1 | call setline(l,'Created Time  : '.strftime('%c'))
let l = l + 1 | call setline(l,'File Name     : '.expand('%'))
let l = l + 1 | call setline(l,'************************************************ */')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'#include <bits/stdc++.h>')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);')
let l = l + 1 | call setline(l,'#define endl ''\n''')
let l = l + 1 | call setline(l,'#define ASB using namespace std; typedef long long ll; namespace AkvicorS {')
let l = l + 1 | call setline(l,'#define ASE } int main() { return AkvicorS::sol(); }')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'ASB')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'int sol(){')
let l = l + 1 | call setline(l,'  FAST_IO;')
let l = l + 1 | call setline(l,'  ')
let l = l + 1 | call setline(l,'  ')
let l = l + 1 | call setline(l,'  ')
let l = l + 1 | call setline(l,'  return 0;')
let l = l + 1 | call setline(l,'}')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'ASE')
let l = l + 1 | call setline(l,'')
exec "21"
endfunc
```


