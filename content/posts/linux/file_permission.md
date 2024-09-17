---
title: "File Permission"
date: 2022-08-19 15:21:16 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 修改文件或目录所有者

```bash
# 修改文件的所有者为root
chown root <file>
# 修改文件的所有者和所有组为root
chown root: <file>
# 修改文件的所有者为root，所有组为akvicor
chown root:akvicor <file>
# 修改文件的所有组为root
chown :root <file>
```

## 修改文件或目录的访问权限

```bash
# chmod <xxx> <file/dir>
chmod 777 file
# chmod <u/g/o/a><+/-><r/w/x> <file/dir>
chmod u+x file
```

### 目标

- `u`: 用户
- `g`: 组
- `o`: 其他用户
- `a`: 全部

### 操作

- `+`: 加
- `-`: 减

### 权限

- `r`: 读
- `w`: 写
- `x`: 执行

### 特殊权限

- `s`: ->`u`组，简称SUID的特殊权限。当执行该文件时将具有该文件所有者的权限。
- `s`: ->`g`组，简称SGID的特殊权限。在该目录下创建的文件和目录都属于该目录所属的组
- `t`: ->`o`组，简称SBIT的特殊权限。只能让所属主以及root可以删除/移动/重命名该目录下的文件

其中sudo命令就是通过`u`组的`s`权限实现的。

## sudo原理

### 源码

```c
#include <stdio.h>
#include <unistd.h>

int main(int argc, char* argv[]) {
    printf("ruid: %d\n",getuid());
    printf("euid: %d\n",geteuid());

    if(execvp(argv[1], argv+1) == -1) {
        perror("execvp error");
    };
    return 0;
}
```

### 编译

查看文件权限

```
-rwxr-xr-x  1 vivc vivc  17K Aug 19 16:32 main
```

### 运行

可以看到文件ruid和euid均为自己

```
ruid: 1001
euid: 1001
Segmentation fault
```

### 修改文件所有者

```bash
sudo chown akvicor main
```

### 查看文件权限

```
-rwxr-xr-x  1 akvicor vivc  17K Aug 19 16:32 main
```

### 运行

可以看到文件ruid和euid均为自己

```
ruid: 1001
euid: 1001
Segmentation fault
```

### 添加s权限

```bash
chmod u+s main
```

### 运行

可以看到ruid为自己，euid则与文件拥有者相同，此时这个程序运行时将具有euid用户的权限

```
ruid: 1001
euid: 1000
Segmentation fault
```

### 测试权限

由于代码中固定需要两个参数，因此使用`rm file`测试。

```
-rw------- 1 akvicor akvicor 0 Aug 19 16:49 file
```

`file`文件属于`akvicor`，只有`akvicor`有权限删除。而`main`程序由于具有`s`特殊权限且属于用户`akvicor`，因此通过`main`来执行rm命令便可以删除`file`文件

```
./main rm file
```

