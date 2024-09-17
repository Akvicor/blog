---
title: "zsh"
date: 2022-08-09 16:34:16 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## oh-my-zsh主题显示执行时间

oh-my-zsh主题显示执行时间

进入主题目录`~/.oh-my-zsh/themes`

在想要修改的主题文件中添加如下代码：

```
function preexec() {
  timer=${timer:-$SECONDS}
}

function precmd() {
  if [ $timer ]; then
    timer_show=$(($SECONDS - $timer))
    if [[ $timer_show -ge $min_show_time ]]; then
      RPROMPT='%{$fg_bold[red]%}(${timer_show}s)%f%{$fg_bold[white]%}[%*]%f %{$reset_color%}%'
    else
      RPROMPT='%{$fg_bold[white]%}[%*]%f'
    fi
    unset timer
  fi
}

autoload -Uz add-zsh-hook
add-zsh-hook preexec preexec
add-zsh-hook precmd precmd
```

