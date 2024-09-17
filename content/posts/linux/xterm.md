---
title: "Xterm"
date: 2023-02-15 12:10:02 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

```shell
vim ~/.Xresource
```

填写配置

```
xterm.termName: xterm-256color
xterm*locale:true
xterm.utf8: true
xterm*utf8Title: true

!fix alt key input 
!xterm*eightBitInput: false
!xterm*altSendsEscape: true

!xterm*scrollBar: true
!xterm*rightScrollBar: true
xterm*SaveLines: 4096

!xterm*background: black
!xterm*foreground: green

xterm*printAttributes: 0
xterm*printerCommand: cat > ~/xtermdump

!xterm*VT100.translations: #override <Btn1UP>: select-end(PRIMARY, CLIPBOARD, CUT_BUFFER0)
xterm*VT100.translations: #override \
        Ctrl <KeyPress> V: insert-selection(CLIPBOARD,PRIMARY,CUT_BUFFER0) \n\
        <BtnUp>: select-end(CLIPBOARD,PRIMARY,CUT_BUFFER0) \n\
        Ctrl <KeyPress> P: print() \n

xterm*faceName: Ubuntu Bold:antialias=True:pixelsize=12
xterm*faceNameDoublesize:Noto Sans Mono CJK SC:antialias=True:pixelsize=12
```



