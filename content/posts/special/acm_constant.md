---
title: "ACM卡常数(各种玄学优化)"
date: 2019-05-08T21:16:14Z
draft: false
categories: [Special]
tags: []
card: false
weight: 0
---

**首先声明，本博文部分内容仅仅适用于ACM竞赛，并不适用于NOIP与OI竞赛，违规使用可能会遭竞赛处理，请慎重使用！遭遇任何情况都与本人无关哈=7=**

我也不想搞得那么严肃的，但真的有些函数在NOIP与OI竞赛中有相关规定不能使用，详细我也不知道各位要了解请自行去找比赛要求咯，当然在ACM竞赛中，没有限制函数，所以所有内容都适用于ACM竞赛。

那么什么是卡常数呢，简单来说就是你和某神犇算法思路一样，结果他的AC了，你的TLE，复杂来说就是程序被卡常数，一般指程序虽然渐进复杂度可以接受，但是由于实现/算法本身的时间常数因子较大，使得无法在OI/ACM等算法竞赛规定的时限内运行结束。

下面就是介绍各种各样的非（花）常（里）实（胡）用（哨）的优化方法的，若本文某些地方有错误或不明确的地方还请指出。=7=

<!--more-->

## 常见

```c++
~i // i是-1则返回0，否则返回其他值

i=-1  ~i==0
i=other ~i!=0
```

```c++
i^1 // 常用与成对的索引，比如网络流中的 正边 和 反边

i=0 i^1=1
i=1 i^1=0

i=2 i^1=3
i=3 i^1=2
```

## 优化I/O

网上有很多说关于cin和scanf的介绍，以及关闭流输入等等优化方法，但这些都还是有可能成为卡常数的地方，那么这个时候，我们就可以自己写输出输出函数了。

**下面一个简单的对读入数字的优化：**

```c++
inline void read(int &sum) {
    char ch = getchar();
    int tf = 0;
    sum = 0;
    while((ch < '0' || ch > '9') && (ch != '-')) ch = getchar();
    tf = ((ch == '-') && (ch = getchar()));
    while(ch >= '0' && ch <= '9') sum = sum * 10+ (ch - 48), ch = getchar();
    (tf) && (sum =- sum);
}
```

因为getchar()是比scanf和cin快很多的，所以可以用这种方式优化很多，当然也可以写对其他各种类型输入的优化。

然后就是进阶版优化，cstdio库里面有一个非常快而且和freopen和fopen完美兼容的函数就是fread，而且是整段读取，函数原型为：

```c++
1 size_t fread(void *buffer,size_t size,size_t count,FILE *stream);
```

作用：从stream中读取count个大小为size个字节的数据，放到数组buffer中，返回成功了多少个大小为为size个字节的数据。

**所以我们的代码可以更加优化为：**

```c++
inline char nc() {
    static char buf[1000000], *p1 = buf, *p2 = buf;
    return p1 == p2 && (p2 = (p1 = buf) + fread (buf, 1, 1000000, stdin), p1 == p2) ? EOF : *p1++;
}

//#define nc getchar
inline void read(int &sum) {
    char ch = nc();
    int tf = 0;
    sum = 0;
    while((ch < '0' || ch > '9') && (ch != '-')) ch = nc();
    tf = ((ch == '-') && (ch = nc()));
    while(ch >= '0' && ch <= '9') sum = sum * 10+ (ch - 48), ch = nc();
    (tf) && (sum =- sum);
}
```

但要注意，由于这种方法是整段读取的，这也造就了它两个巨大的Bug：

1. 不能用键盘输入。数据还没输入，程序怎么整段读取。如果你需要在电脑上用键盘输入调试，请把第5行的注释取消。
2. 不能和`scanf`，`getchar`等其他读入方法混合使用。因为`fread`是整段读取的，也就是说所有数据都被读取了，其他函数根本读取不到任何东西(只能从你的读取大小后面开始读)，因此，所有类型的变量读入都必须自己写，上面的`read`函数只支持`int`类型。

| #               | Language        | [0,2) | [0,8) | [0,2^{15})) | [0,2^{31}) | [0,2^{63}) |
| --------------- | --------------- | ----- | ----- | ----------- | ---------- | ---------- |
| fread           | G++ 5.4.0 (-O2) | 13    | 13    | 39          | 70         | 111        |
| getchar         | G++ 5.4.0 (-O2) | 58    | 73    | 137         | 243        | 423        |
| cin（关闭同步） | G++ 5.4.0 (-O2) | 161   | 147   | 205         | 270        | 394        |
| cin             | G++ 5.4.0 (-O2) | 442   | 429   | 706         | 1039       | 1683       |
| scanf           | G++ 5.4.0 (-O2) | 182   | 175   | 256         | 368        | 574        |

`fread`以压倒性的优势碾压了其他所有方法，还可以注意到关流同步的cin比scanf快，关于为什么不使用位运算的问题下面会说。

然后就是输出的优化，同理，putchar()会比printf快，所以，输出数字可以优化成：

```c++
// 优化前输出1-10000000：4.336秒
// 优化后输出1-10000000：1.897秒
void print( int k ){
    num = 0;
    while( k > 0 ) ch[++num] = k % 10, k /= 10;
    while( num ) 
        putchar( ch[num--]+48 );
    putchar( 32 );

}
```

如果输出负数以及其他，就自己写一个或者百度啦，我这里就不贴了。其实大多数还是对读入进行优化，输出一般用printf就可以了。

## 位运算

很多人都肯定很喜欢用位运算吧，因为觉得位运算是基于二进制操作，肯定比普通加减乘除快很多，但是真的是所有的位运算操作都比常规操作快么。

### 乘和除的位运算

```c++
x << 1;
x *= 2;
```

例如上面这两句，都是把x乘2，但真的用位运算会快么，其实他们理论上是一样的，在被g++翻译成汇编后，两者的语句都是

```assembly
addl    %eax, %eax1
```

它等价于 x = x + x。所以在这里位运算并没有任何优化。那么把乘数扩大呢，比如乘10，x *= 10的汇编语言为

```assembly
leal    (%eax,%eax,4), %eax
addl    %eax, %eax
```

翻译过来就是

```c++
x = x + x*4;
x = x + x;
```

而那些喜欢用(x << 3 + x << 1)的人自己斟酌！

 

但是位运算在某些地方是非常有用的，比如除法，右移的汇编代码为

```assembly
movl    _x, %eax
sarl    %eax
movl    %eax, _x
movl    _x, %eax
```

而除二的汇编代码为

```assembly
movl    _x, %eax
movl    %eax, %edx  //(del)
shrl    $31, %edx  //(del)
addl    %edx, %eax  //(del)
sarl    %eax
movl    %eax, _x
movl    _x, %eax
```

可以看到，右移会比除快很多。

### %2和&1

这个其实可想而知&1快，还是看下汇编代码吧，%2的汇编代码为

```assembly
movl    _x, %eax
movl    $LC0, (%esp)
movl    %eax, %edx  //(del)
shrl    $31, %edx  //(del)
addl    %edx, %eax  //(del)
andl    $1, %eax
subl    %edx, %eax  //(del)
movl    %eax, 4(%esp)
movl    %eax, _x
```

&1的汇编代码为

```assembly
movl    _x, %eax
movl    $LC0, (%esp)
andl    $1, %eax
movl    %eax, 4(%esp)
movl    %eax, _x
```

------

### ^和swap

最开始学C语言两个变量交换都是先学三变量交换法，再学^这种操作，下面是(a ^= b ^= a ^= b)的汇编代码

```assembly
movl    _b, %edx
movl    _a, %eax
xorl    %edx, %eax
xorl    %eax, %edx
xorl    %edx, %eax
movl    %eax, _a
xorl    %eax, %eax
movl    %edx, _b
```

再来看看(int t = a;a = b,b = t;)的汇编代码

```assembly
movl    _a, %eax
movl    _b, %edx
movl    %eax, _b
xorl    %eax, %eax
movl    %edx, _a
```

谁慢谁快一眼就知道了，以后swap再无Xor。

### 其他位运算技巧

网上有很多奇奇怪怪的位运算技巧，但有一些真的令人很无语，没有优化不说，大大降低了代码可读性，在我看来，都是些花里胡哨的操作，比如取绝对值(n ^ (n >> 31)) - (n >> 31)，取两个数的最大值b & ((a - b) >> 31) | a & ( ~(a - b) >> 31)，取两个数的最小值a & ((a - b) >> 31) | b & ( ~(a-b) >> 31 )。恕我愚钝，这些代码一眼看上去根本不知道在干嘛，还有那个取绝对值的和abs(x)，谁快都不用说了。

但是位运算还是有很多好（骚）操作的，例如：

lowbit函数 ： x & (-x)
判断是不是2的幂：x > 0 ? ( x & (x - 1)) == 0 : false

emmm……还有很多，我就不介绍了（我就知道这两个=7=）

## 条件判断优化

acm不可避免会有条件语句，if-else也好，?:也好，switch也好，那么问题来了，最后用哪种呢，让我们一一道来。

### if和?:

网上很多说if比?:慢，但是其实不是这样的，二者的汇编除了文件名不一样其他都一模一样。其实不是?:比if快而是?:比if-else快。

有什么区别吗？你需要先弄清楚if-else的工作原理。
if就像一个铁路分叉道口，在CPU底层这种通讯及其不好的地方，在火车开近之前，鬼知道火车要往哪边开，那怎么办？猜！
如果猜对了，它直接通过，继续前行。
如果猜错了，车头将停止，倒回去，你将铁轨扳至反方向，火车重新启动，驶过道口。
如果是第一种情况，那很好办，那第二种呢？时间就这么浪过去了，假如你非常不走运，那你的程序就会卡在停止-回滚-热启动的过程中。
上面猜的过程就是**分支预测**。
虽然是猜，但编译器也不是随便乱猜，那怎么猜呢？答案是分析之前的运行记录。假设之前很多次都是true，那这次就猜true，如果最近连续很多次都是false，那这次就猜false。
但这一切都要看你的CPU了，因此，一般把容易成立的条件写在前面判断，把不容易成立的条件放在else那里。
但是?:消除了分支预测，因此在布尔表达式的结果近似随机的时候?:更快，否则就是if更快啦。

 

### 分支预测优化

gcc存在内置函数：__builtin_expect(!!(x), tf)，他不会改变x的值，仅仅只是减少跳转次数，当tf为true时表示x非常可能为true，反之同理。

用法就是if(__builtin_expect(!!(x),0)) 或者把0换为1，这样在if猜的时候就会优先猜x为true或是false，达到优化效果。

### switch和if-else

这个东西还是有必要提一下，当switch没有default的时候，switch会比if-else快，因为他是直接跳转而不是逐条判断，但加了default之后，switch也就变成了无脑判断模式，至于为什么会这样，各位就自行研究咯=7=

 

### 短路运算符

我们知道&&和||是两个短路运算符，什么叫短路运算符，就是一旦可以确定了表达式的真假值时候，就直接返回真假值了，比如下面代码

```c++
int n = 0;

n && ++n;

//这里n的值还是0

!n || ++n;

//这里n的值还是0
```

但是上面的两句代码等同于什么呢？等于

```c++
int n = 0;

if(n){
    ++n;
}

if(!(!n)){
    ++n;
}
```

利用这个特色（你才特色），我们有些时候就可以不需要在做if的无脑判断了，也就是

- if(A) B; → (A)&&(B)

 

- if(A) B; else C; → A&&(B,1)||C

 

但这些并不是短路运算符的精髓，短路运算符的精髓不仅在于优化时间，更是可以防止程序出错。



```c++
double t = rand();
if (t / RAND_MAX < 0.2 && t != 0)
    printf ("%d", t);

double t = rand();
if (t != 0 && t / RAND_MAX < 0.2)
    printf ("%d", t);
```

 

这两种判断，谁快谁慢。但对于CPU来说很有区别。第一段代码中的t/RAND_MAX<0.2为true的概率约为 20%，但t!=0为true的概率约为1/RAND_MAX，明显小于20%

因此，如果把计算一个不含逻辑运算符布尔表达式的计算次数设为 1 次，设计算了 X 次，则对于第 1 段代码，X 的数学期望为  6/5 次，但对于第二段代码，X 的数学期望2*(RAND_MAX-1) / RAND_MAX为 ，远远大于第一段代码。

不仅不同位置会优化时间，更是会防止程序错误，例如kuangbin搜索专题有题是Catch the Cow，就是搜索，不过判断走没走过得判断vis[n]和n < 1e6，我最最开始写的vis[n] && n < 1e6，提交上去RE了，看了很久才发现是这里的原因，得先判断n < 1e6，再做下一步操作。

所以， 遇到A&&B时，优先把可能为false的表达式放在前面。遇到A||B时，优先把可能为true的表达式放在前面。但也不一定是绝对这样，还得结合题目。

### 布尔表达式和逗号运算符

很多人喜欢用if(x == true)这种形式，但其实if(x)就行了，在可读性等方面都没有变化。而且不要开bool数组，int是最快的（原因暂时不知道）。

逗号运算符若干条语句可以通过逗号运算符合并成一条语句。 例如t=a;a=b;b=t;可以写成t=a,a=b,b=t;有什么用吗？它的返回值。
int x=(1,2,3,4,5);
猜一猜，上面的语句执行完后x的值是多少？ 答案是 5 没错，逗号运算符的返回值就是最后一个的值。而且逗号表达式比分号快很多很多，真的。

## 卡编译

### C++内联函数`inline`：

由编译器在编译时会在主程序中把函数的内容直接展开替换，减少了内存访问，但是这并不是适用于各种复杂以及递归式的函数，复杂函数编译器会自动忽略inline

```c++
int max(int a, int b){return a>b?a:b;}//原函数
inline int max(int a, int b){return a>b?a:b;}//直接加inline就好了。
```

### CPU寄存器变量`register`：

对于一些频繁使用的变量，可以声明时加上该关键字，运行时**可能**会把该变量放到CPU寄存器中，只是可能，因为寄存器的空间是有限的，不保证有效。特别是你变量多的时候，一般还是丢到内存里面的。 
比较下面两段程序：

```c++
register int a=0;
for(register int i=1;i<=999999999;i++)a++;

int a=0;
for(int i=1;i<=999999999;i++)a++;
```

优化：0.2826 second 
不优化：1.944 second

## 卡算法

### 取模优化

```c++
//设模数为 mod
inline int inc(int x,int v,int mod){x+=v;return x>=mod?x-mod:x;}//代替取模+
inline int dec(int x,int v,int mod){x-=v;return x<0?x+mod:x;}//代替取模-
```


### 加法优化

用++i代替i++，后置++需要保存临时变量以返回之前的值，在 STL 中非常慢。



### 结构优化

如果要经常调用a[x],b[x],c[x]这样的数组，把它们写在同一个结构体里面会变快一些，比如f[x].a, f[x].b, f[x].c 
指针比下标快，数组在用方括号时做了一次加法才能取地址！所以在那些计算量超大的数据结构中，你每次都多做了一次加法！！！在 64 位系统下是 long long 相加，效率可想而知。


### STL优化

STL快但是也包含了很多可能你用不到的东西，所以最快的就是你自己手写STL=7=，反正我写不来。


### 循环展开


```c++
 void Init(int *d, int n){
    for(int i = 0; i < n; i++)
        d[i] = 0;
}


void Init(int *d, int n){
    int il   
    for(int i = 0; i < n; i+= 4){    //每次迭代处理4个元素
        d[i] = 0;
        d[i + 1] = 0;
        d[i + 2] = 0;
        d[i + 3] = 0;
    }
    for(; i < n; i++)//将剩余未处理的元素再依次初始化
        d[i] = 0;
}
```

都是同一个操作，但你们觉得谁快呢，用下面的比第一段代码快了不止一倍，循环展开也许只是表面，在缓存和寄存器允许的情况下一条语句内大量的展开运算会刺激 CPU 并发

- 减少了不直接有助于程序结果的操作的数量，例如循环索引计算和分支条件。
- 提供了一些方法，可以进一步变化代码，减少整个计算中关键路径上的操作数量。

---

好像没什么要讲的了呢，网上还有一些很邪门的优化方式，我觉得就没必要了，能大致知道一些优化流程就行了，比如读入还有mmap但用这个不是很了解的话可能还会用出事，所以别没必要那么追求极限了。自己觉得讲的还是挺多挺全面的，若是哪里有错误或者没讲到的地方还请指出。

