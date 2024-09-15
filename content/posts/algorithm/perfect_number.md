---
title: "完全数"
date: 2019-04-20T22:47:56Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

**完全数**，又称**完美数**或**完备数**，是一些特殊的[自然数](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%84%B6%E6%95%B0)：它所有的真[因子](https://zh.wikipedia.org/wiki/%E5%9B%A0%E5%AD%90)（即除了自身以外的约数）的和，恰好等于它本身，完全数不可能是[楔形数](https://zh.wikipedia.org/wiki/%E6%A5%94%E5%BD%A2%E6%95%B8)。

例如：第一个完全数是6，它有约数1、2、3、6，除去它本身6外，其余3个数相加，1+2+3＝6，恰好等于本身。第二个完全数是28，它有约数1、2、4、7、14、28，除去它本身28外，其余5个数相加，1+2+4+7+14＝28，也恰好等于本身。后面的数是[496](https://zh.wikipedia.org/wiki/496)、[8128](https://zh.wikipedia.org/wiki/8128)。

[十进制](https://zh.wikipedia.org/wiki/%E5%8D%81%E9%80%B2%E4%BD%8D)的5位数到7位数、9位数、11位数、13到18位数等位数都没有完全数，它们不是[亏数](https://zh.wikipedia.org/wiki/%E4%BA%8F%E6%95%B0)就是[过剩数](https://zh.wikipedia.org/wiki/%E9%81%8E%E5%89%A9%E6%95%B8)。

<!--more-->

## 完全数的发现

古希腊数学家欧几里得是通过{{<latex>}}2^{n-1}\times(2^n-1){{</latex>}}的表达式发现前四个完全数的。

当{{<latex>}}n=2 : 2^1\times(2^2-1)=6{{</latex>}}

当{{<latex>}}n=3 : 2^2\times(2^3-1)=28{{</latex>}}

当{{<latex>}}n=5 : 2^4\times(2^5-1)=496{{</latex>}}

当{{<latex>}}n=7 : 2^6\times(2^7-1)=8128{{</latex>}}

一个偶数是完美数，当且仅当它具有如下形式：{{<latex>}}{\displaystyle 2^{n-1}(2^{n}-1)}{{</latex>}}，其中{{<latex>}}{\displaystyle 2^{n}-1}{{</latex>}}是素数，此事实的充分性由欧几里得证明，而必要性则由[欧拉](https://zh.wikipedia.org/wiki/%E6%AD%90%E6%8B%89)所证明。

比如，上面的{{<latex>}}{\displaystyle 6}{{</latex>}}和{{<latex>}}{\displaystyle 28}{{</latex>}}对应着{{<latex>}}{\displaystyle n=2}{{</latex>}}和{{<latex>}}{\displaystyle 3}{{</latex>}}的情况。我们只要找到了一个形如{{<latex>}}{\displaystyle 2^{n}-1}​{{</latex>}}的[素数](https://zh.wikipedia.org/wiki/%E7%B4%A0%E6%95%B0)（即[梅森素数](https://zh.wikipedia.org/wiki/%E6%A2%85%E6%A3%AE%E7%B4%A0%E6%95%B0)），也就知道了一个偶完美数。

尽管没有发现奇完全数，但是当代数学家[奥斯丁·欧尔](https://zh.wikipedia.org/wiki/%E5%A5%A5%E6%96%AF%E4%B8%81%C2%B7%E6%AC%A7%E5%B0%94)证明，若有奇完全数，则其形式必然是{{<latex>}}{\displaystyle 12p+1}{{</latex>}}或{{<latex>}}{\displaystyle 36p+9}{{</latex>}}的形式，其中{{<latex>}}{\displaystyle p}{{</latex>}}是素数。

首十个完全数是（[![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d8/OEISicon_light.svg/14px-OEISicon_light.svg.png)](https://zh.wikipedia.org/wiki/OEIS) [A000396](https://oeis.org/A000396)）：

1. 6（1位）
2. 28（2位）
3. 496（3位）
4. 8128（4位）
5. 33550336（8位）
6. 8589869056（10位）
7. 137438691328（12位）
8. 2305843008139952128（19位）
9. 2658455991569831744654692615953842176（37位）
10. 191561942608236107294793378084303638130997321548169216（54位）



每一个[梅森素数](https://zh.wikipedia.org/wiki/%E6%A2%85%E6%A3%AE%E7%B4%A0%E6%95%B0)给出一个偶完全数；反之，每个偶完全数给出一个梅森素数，这结果称为[欧几里得－欧拉定理](https://zh.wikipedia.org/wiki/%E6%AD%90%E5%B9%BE%E9%87%8C%E5%BE%97%EF%BC%8D%E6%AD%90%E6%8B%89%E5%AE%9A%E7%90%86)。到 2018 年 1 月为止，共发现了 50 个完全数，且都是偶数。最大的已知完全数为{{<latex>}}2^{77232916}\times(2^{77232917}-1){{</latex>}}共有 {{<latex>}}{\displaystyle 46498850}{{</latex>}} 位数[[1\]](https://zh.wikipedia.org/wiki/%E5%AE%8C%E5%85%A8%E6%95%B0#cite_note-1)。

## 性质

以下是目前已发现的完全数共有的性质。

- 偶完全数都是以6或28结尾。
- 在[十二进制](https://zh.wikipedia.org/wiki/%E5%8D%81%E4%BA%8C%E8%BF%9B%E5%88%B6)中，除了6跟28以外的偶完全数都以54结尾，甚至，除了6, 28, 496以外的偶完全数都以054或854结尾。而如果存在奇完全数，她在十二进制中必定以1, 09, 39, 69或99结尾。
- 除6以外的偶完全数，把它的各位数字相加，直到变成个位数，那么这个个位数一定是1[[注 1\]](https://zh.wikipedia.org/wiki/%E5%AE%8C%E5%85%A8%E6%95%B0#cite_note-2)：

{{<latex>}}28→2+8=10→1+0=1{{</latex>}}

- 所有的偶完全数都可以表达为2的一些连续正整数次幂之和，从{{<latex>}}{\displaystyle 2^{p-1}}{{</latex>}}到{{<latex>}}{\displaystyle 2^{2p-2}}{{</latex>}}：

{{<latex>}}6=2^1+2^2{{</latex>}}
{{<latex>}}28=2^2+2^3+2^4{{</latex>}}
{{<latex>}}496=2^4+2^5+2^6+2^7+2^8{{</latex>}}
{{<latex>}}8128=2^6+2^7+...+2^{12}{{</latex>}}

- 每个偶完全数都可以写成连续自然数之和[[注 2\]](https://zh.wikipedia.org/wiki/%E5%AE%8C%E5%85%A8%E6%95%B0#cite_note-3)：

{{<latex>}}6=1+2+3{{</latex>}}

{{<latex>}}28=1+2+3+4+5+6+7{{</latex>}}

{{<latex>}}496=1+2+3+4+…+30+31{{</latex>}}

* 除6以外的偶完全数，还可以表示成连续奇[立方数](https://zh.wikipedia.org/wiki/%E7%AB%8B%E6%96%B9%E6%95%B0)之和（被加的项共有{{<latex>}}{\displaystyle {\sqrt {2^{p-1}}}}{{</latex>}})[[注 3\]](https://zh.wikipedia.org/wiki/%E5%AE%8C%E5%85%A8%E6%95%B0#cite_note-4)：

{{<latex>}}28=1^3+3^3{{</latex>}}

{{<latex>}}496=1^3+3^3+5^3+7^3{{</latex>}}

- 每个完全数的所有约数（包括本身）的倒数之和，都等于2：（这可以用通分证得。因此每个完全数都是[欧尔调和数](https://zh.wikipedia.org/wiki/%E6%AD%90%E7%88%BE%E8%AA%BF%E5%92%8C%E6%95%B8)。）

{{<latex>}}\frac{1}{1}+\frac{1}{2}+\frac{1}{3}+\frac{1}{6}=\frac{6+3+2+1}{6}=2​{{</latex>}}

{{<latex>}}\frac{1}{1}+\frac{1}{2}+\frac{1}{4}+\frac{1}{7}+\frac{1}{14}+\frac{1}{28}=\frac{28+14+7+4+2+1}{28}=2{{</latex>}}

它们的[二进制](https://zh.wikipedia.org/wiki/%E4%BA%8C%E8%BF%9B%E5%88%B6)表达式也很有趣：（因为偶完全数形式均如{{<latex>}}2^{n-1}(2^n-1){{</latex>}})

{{<latex>}}(6)_{10}=(110)_2​{{</latex>}}

{{<latex>}}(28)_{10}=(11100)_2{{</latex>}}

{{<latex>}}(496)_{10}=(111110000)_2{{</latex>}}

