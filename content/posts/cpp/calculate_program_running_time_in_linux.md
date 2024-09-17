---
title: "Linux/Unix 环境下实现精确计算程序运行的时间"
date: 2019-05-27T20:40:06Z
draft: false
categories: [C++]
tags: []
card: false
weight: 0
---

写程序时，程序的运行效率很重要，其往往是评价程序优劣性的直接标准。程序运行效率的最简单方法就是计算程序的运行时间。为了提高程序效率，使用适当的方法对程序的各个部分进行运行时间的计算是很有必要的。

<!--more-->

在 Linux/Unix 环境下，计算 C 程序运行时间可以通过以下三个函数来实现：

`clock()`、`time()`、`gettimeofday()`

 - gettimeofday()函数的精度最高，为微秒级别；
 - clock()函数的精度次之，为 10ms 级别；
 - time()函数的精度最低，为 1s 级别。

## clock() 函数

`clock()`函数是 ANSI C 的标准库函数，是 C/C++ 十分常用的计时函数，其声明定义在 time.h 头文件中：

```c++
clock_t clock( void ); 
```
此函数返回处理器调用某个进程或函数所花费的时间的近似值，准确来说就是返回从“开启这个程序进程”到“程序中调用`clock()`函数”时这之间的 CPU 时钟计时单元（clock tick）数，在 MSDN 中称之为挂钟时间（wal-clock）。如果无法得到处理器时间，则返回数值 -1（转换为 time_t 类型）。时钟计时单元（clock tick）的时间长短是由CPU控制的。一个 clock tick 不是 CPU 的一个时钟周期，而是 C/C++ 的一个基本计时单位。返回类型为`clock_t`，是用来保存时间的数据类型，在 time.h 文件中，我们可以找到对它的定义：

```c++
#ifndef _CLOCK_T_DEFINED
typedef long clock_t;
#define _CLOCK_T_DEFINED
#endif
```

很明显，`clock_t`本质上是一个长整形数。

以上可知`clock()`函数返回的是时钟计时单元数（俗称硬件滴答数），要换算成秒或者毫秒，需要用到`CLOCKS_PER_SEC`常量（或者`CLK_TCK`常量，两者其实一样），此常量在 time.h 文件中定义，用来表示一秒钟会有多少个时钟计时单元。在不同的系统中`CLOCKS_PER_SEC`常量的值通常不一样，比如在本人的 linux 系统下，其定义如下：

```c++
#define CLOCKS_PER_SEC  1000000l（最后面的是字母l）
```

而在 Windows 系统下的 VC6.0 中，其定义为：

```c++
#define CLOCKS_PER_SEC ((clock_t)1000)
```

这表示硬件滴答 1000 下是 1 秒，也即可以看到每过千分之一秒（1毫秒），调用`clock()`函数返回的值就加 1。

因此，要计算运行某程序所需的时间只需要利用`clock()`函数得到运行此程序所消耗的钟计时单元数，然后再除以`CLOCKS_PER_SEC`即可。

比如可以使用公式 clock()/CLOCKS_PER_SEC 来计算一个进程自身的运行时间：

```c++
void elapsed_time(){  
    printf("Elapsed time:%u secs.\n",clock()/CLOCKS_PER_SEC);  
}
```

下面用`clock()`函数来计算运行一个循环或者处理其它事件到底花了多少时间：

```c++
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int main(void) {
    long i = 10000000L;
    clock_t start, finish;
    double duration;
    /* 测量一个事件持续的时间*/
    printf( "Time to do %ld empty loops is ", i) ;
    start = clock();
    while( i-- );
    finish = clock();
    duration = (double)(finish - start) / CLOCKS_PER_SEC;
    printf( "%f seconds\n", duration );
    return 0;
}
```

在本人的机器上（Linux系统），运行结果如下：
```
Time to do 10000000 empty loops is 0.03000 seconds
```

对于`clock()`函数来说，需要注意以下几点：

它返回的是 CPU 耗费在本程序上的时间。也就是说，途中 sleep 的话，由于 CPU 资源被释放，那段时间将不被计算在内。因此，若函数中存在`sleep()`函数，则`sleep()`函数消耗的时间将不包含在内；

得到的返回值其实就是耗费在本程序上的 CPU 时间片的数量，也就是 Clock Tick 的值。该值必须除以`CLOCKS_PER_SEC`这个宏值，才能最后得到以秒为单位的运行时间。在 POSIX 兼容系统中，`CLOCKS_PER_SEC`的值为 1,000,000 的，也就是 1MHz。

这个函数的精度不适很高，大约为 10ms，也有说是 1ms，但是在本人机器上达不到那么高。低于精度的程序全部输出 0ms。像计算`printf()`之类的调用时间是不可能实现的，因为`printf()`的速度太快了，基本上和`clock()`的速度一样。所以，此函数适合用于计算一些大型程序，或循环程序。

## time() 函数

`time()`函数来获得当前日历时间（Calendar Time）。所谓的日历时间就是用“从一个标准时间点（一般是1970年1月1日0时0分0秒）到此时的时间经过的秒数”来表示的时间。其原型为：

```c++
time_t time(time_t * timer); 
```

如果 timer 不是 NULL，则返回值还存放在 timer 中。如果遇到错误，则返回 -1（转换成 time_t 类型）。time_t 是一个 long 类型。如果你已经声明了参数 timer，你可以从参数 timer 返回现在的日历时间，同时也可以通过返回值返回现在的日历时间，即从一个时间点（例如：1970年 1月1日0时0分0秒）到现在此时的秒数。如果参数为空（NULL），函数将只通过返回值返回现在的日历时间，比如下面这个例子用来显示当前的日历时间：

```c++
#include <time.h> 
#include <stdio.h> 
#include <dos.h> 
int main() {
    time_t t;
    t=time();
    printf("The number of seconds since January 1,1970 is %ld",t);
    return 0; 
}
```

也可以利用time()函数计算某段程序的运行时间：

```c++
#include<time.h>
#include<stdio.h>
#include<stdlib.h>

int main() {
    time_t t1,t2;
    time(&t1);
    //此处放置要测试的代码
    sleep(10);//延时 1 秒
    time(&t2);
    printf("%ld %ld %lds\n",t1,t2,t2-t1);
    return 0; 
}
```

在本人机器上的运行结果为：

```
1361997962 1361997972 10s
```

需要注意的是，`time()`得到的时间的精度只有 1s。

## gettimeofday() 函数

gettimeofday()的函数原型为：

```c++
#include<sys/time.h>
int gettimeofday(struct timeval*tv, struct timezone *tz )
```

`gettimeofday()`会把目前的时间用 tv 结构体返回，当地时区的信息则放到 tz 所指的结构中。其结构体定义为：

```c++
struct  timeval {
    long  tv_sec; /*秒*/
    long  tv_usec; /*微妙*/
}；

struct  timezone{
    int tz_minuteswest; /*和greenwich 时间差了多少分钟*/
    int tz_dsttime; /*type of DST correction*/
};
```

在`gettimeofday()`函数中 tv 或者 tz 都可以为空。如果为空则就不返回其对应的结构体。函数执行成功后返回 0，失败后返回 -1，错误代码存于 errno 中。

程序实例为：

```c++
#include<stdio.h>
#include<sys/time.h>
#include<unistd.h>
int main() {
    struct  timeval    tv;
    struct  timezone   tz;

    gettimeofday(&tv,&tz);
    printf(“tv_sec:%d\n”,tv.tv_sec);
    printf(“tv_usec:%d\n”,tv.tv_usec);
    printf(“tz_minuteswest:%d\n”,tz.tz_minuteswest);
    printf(“tz_dsttime:%d\n”,tz.tz_dsttime);
    return 0;
}
```

在使用`gettimeofday()`函数时，第二个参数一般都为空，因为我们一般都只是为了获得当前时间，而不用获得 timezone 的数值。

也可利用此函数计算某段程序的运行时间：

```c++
#include <stdio.h>
#include <sys/time.h>
int main() {
    struct  timeval  start;
    struct  timeval  end;
    unsigned long timer;

    gettimeofday(&start,NULL);
    printf("hello world!\n");
    gettimeofday(&end,NULL);
    timer = 1000000 * (end.tv_sec-start.tv_sec)+ end.tv_usec-start.tv_usec;
    printf("timer = %ld us\n",timer);
    return 0;
}
```

```c++
    timeval start{};
    timeval end{};
    unsigned long long timer;

    gettimeofday(&start, nullptr);

    /********************************/
    for (int n = 0; n < 200; ++n) {
        trial_divisio_fac(n);
    }
    /********************************/

    gettimeofday(&end, nullptr);

    timer = 1000000 * (end.tv_sec - start.tv_sec) + end.tv_usec - start.tv_usec;

    printf("timer = %llu us\n", timer);
```

`gettimeofday()`函数的计算精度为微秒，精度已经很高了。


