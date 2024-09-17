---
title: "GDB Usage"
date: 2023-02-15 11:20:25 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 连接远程调试

```
target remote localhost:1234
```

## 查看内存

```
x/<n/f/u> <addr>

x /8xb 0xffff80000002fff0
# 以16进制方式查看0xffff80000002fff0处8字节内容
```

### n

从当前地址往后请求的字节数，如果不指定的话，GDB默认是4个bytes。

### f

- `x` 按十六进制格式显示变量
- `d` 按十进制格式显示变量
- `u` 按十十进制格式显示无符号变量
- `o` 按十八进制格式显示变量
- `t` 按十二进制格式显示变量
- `a` 按十十六进制格式显示变量
- `i` 指令地址格式
- `c` 按字符格式显示变量
- `f` 按浮点数格式显示变量

### u

表示一个地址单元的长度

- `b` 表示单字节
- `h` 表示双字节
- `w` 表示四字节
- `g` 表示八字节

## 断点

```
break 16          # 在源程序第16行
break func        # 在函数func()入口处
break *address    # 在地址address处
```

## 查看断点信息

```
info break        # 查看断点信息
```

## 清除断点

```
delete n    # 清除n号断点
```

## 运行程序

```
r/run
```

## 单条语句调试

```
n/next
```

## 继续运行程序

```
c/continue
```

## 打印变量的值

```
p i
print i
```

## 查看函数堆栈

```
bt
```

## 退出GDB

```
q
```

## 暂停

```
ctrl+c
```

## 单步运行

```
si/ni
```

## 查看当前寄存器和断点

```
info reg/registers
info b/break
```

## 查看当前执行以及之后的汇编指令

```
display /8i $pc   # 8行
display /10i $pc  # 10行
```

## layout

- `layout src`：显示源代码窗口
- `layout asm`：显示汇编窗口
- `layout regs`：显示源代码/汇编和寄存器窗口
- `layout split`：显示源代码和汇编窗口
- `layout next`：显示下一个layout
- `layout prev`：显示上一个layout
- `Ctrl + L`：刷新窗口
- `Ctrl + x，再按1`：单窗口模式，显示一个窗口
- `Ctrl + x，再按2`：双窗口模式，显示两个窗口
- `Ctrl + x，再按a`：回到传统模式，即退出layout，回到执行layout之前的调试窗口。



