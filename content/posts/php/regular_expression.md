---
title: "正则表达式"
date: 2019-02-19T19:00:56Z
draft: false
categories: [PHP]
tags: []
card: false
weight: 0
---

## 什么叫正则表达式

正则表达式是对字符串进行操作的一种逻辑公式，就是用一些特定的字符组合成一个规则字符串，称之为正则匹配模式。

```PHP
$pre = '/apple/';
$str = "apple banna orange";
if (preg_match($pre, $str)) {
    echo 'matched';
}
```

其中字符串`/apple/`就是一个正则表达式，他用来匹配源字符串中是否存在apple字符串。

PHP中使用PCRE库函数进行正则匹配，比如上例中的`preg_match`用于执行一个正则匹配，常用来判断一类字符模式是否存在。

<!--more-->

## 正则表达式的基本语法

PCRE库函数中，正则匹配模式使用分隔符与元字符组成，分隔符可以是非数字、非反斜线、非空格的任意字符。经常使用的分隔符是正斜线(/)、hash符号(#) 以及取反符号(~)，例如：

```php
/foo bar/
#^[^0-9]$#
~php~
```

如果模式中包含分隔符，则分隔符需要使用反斜杠（\）进行转义。

```php
/http:\/\//
```

如果模式中包含较多的分割字符，建议更换其他的字符作为分隔符，也可以采用preg_quote进行转义。

```php
$pre = 'http://';
$pre = '/'.preg_quote($pre, '/').'/';
echo $p;
```

分隔符后面可以使用模式修饰符，模式修饰符包括：i, m, s, x等，例如使用i修饰符可以忽略大小写匹配：

```php
$str = "Https://www.kvxi.org/";
if (preg_match('/https/i', $str)) {
    echo 'matched';
}
```

## 元字符与转义

正则表达式中具有特殊含义的字符称之为元字符，常用的元字符有：

```
\ 一般用于转义字符
^ 断言目标的开始位置(或在多行模式下是行首)
$ 断言目标的结束位置(或在多行模式下是行尾)
. 匹配除换行符外的任何字符(默认)
[ 开始字符类定义
] 结束字符类定义
| 开始一个可选分支
( 子组的开始标记
) 子组的结束标记
? 作为量词，表示 0 次或 1 次匹配。位于量词后面用于改变量词的贪婪特性。 (查阅量词)
* 量词，0 次或多次匹配
+ 量词，1 次或多次匹配
{ 自定义量词开始标记
} 自定义量词结束标记
```

//下面的\s匹配任意的空白符，包括空格，制表符，换行符。[\s]代表非空白符。[\s]+表示一次或多次匹配非空白符。

```php
$p = '/^I[^\s]+(apple|banna)$/';
$str = "I love apple";
if (preg_match($p, $str)) {
    echo 'matched';
}
```

元字符具有两种使用场景，一种是可以在任何地方都能使用，另一种是只能在方括号内使用，在方括号内使用的有：

```
\ 转义字符
^ 仅在作为第一个字符(方括号内)时，表明字符类取反
- 标记字符范围
```

其中^在反括号外面，表示断言目标的开始位置，但在方括号内部则代表字符类取反，方括号内的减号-可以标记字符范围，例如0-9表示0到9之间的所有数字。

//下面的\w匹配字母或数字或下划线。

```php
$pre = '/[\w\.\-]+@[a-z0-9\-]+\.(com|cn)/';
$str = "My Email akvicor@gmail.com";
preg_match($pre, $str, $match);
echo $match[0];
```

## 贪婪模式与懒惰模式

- 贪婪模式

在可匹配与可不匹配的时候，优先匹配

//下面的\d表示匹配数字

```php
$pre = '/\d+\-\d+/';
$str = "My Phone number 21-1234567890";
preg_match($pre, $str, $match);
echo $match[0]; // 结果为：21-1234567890
```

- 懒惰模式

在可匹配与可不匹配的时候，优先不匹配

```php
$pre = '/\d?\-\d?/';
$str = "My Phone number 21-1234567890";
preg_match($pre, $str, $match);
echo $match[0]; // 结果为：1-1
```



当我们确切的知道所匹配的字符长度的时候，可以使用{}指定匹配字符数

```php
$pre = '/\d{2}\-\d{10}/';
$str = "My Phone number 21-1234567890";
preg_match($pre, $str, $match);
echo $match[0]; //结果为：21-1234567890
```

## 使用正则表达式进行匹配

使用正则表达式的目的是为了实现比字符串处理函数更加灵活的处理方式，因此跟字符串处理函数一样，其主要用来判断子字符串是否存在、字符串替换、分割字符串、获取模式子串等。
PHP使用PCRE库函数来进行正则处理，通过设定好模式，然后调用相关的处理函数来取得匹配结果。
preg_match用来执行一个匹配，可以简单的用来判断模式是否匹配成功，或者取得一个匹配结果，他的返回值是匹配成功的次数0或者1，在匹配到1次以后就会停止搜索。

```php
$subject = "abcdef";
$pattern = '/def/';
preg_match($pattern, $subject, $matches);
print_r($matches); // 结果为：Array ( [0] => def )
```

上面的代码简单的执行了一个匹配，简单的判断def是否能匹配成功，但是正则表达式的强大的地方是进行模式匹配，因此更多的时候，会使用模式：

```php
$subject = "abcdef";
$pattern = '/a(.*?)d/';
preg_match($pattern, $subject, $matches);
print_r($matches); // 结果为：Array ( [0] => abcd [1] => bc )
```

通过正则表达式可以匹配一个模式，得到更多的有用的数据。

## 查找所有匹配结果

`preg_match`只能匹配一次结果，但很多时候我们需要匹配所有的结果，`preg_match_all`可以循环获取一个列表的匹配结果数组。

```php
$pre = "|<[^>]+>(.*?)</[^>]+>|i";
$str = "<b>example: </b><div align=left>this is a test</div>";
preg_match_all($pre, $str, $matches);
print_r($matches);
```

可以使用`preg_match_all`匹配一个表格中的数据：

```php
$pre = "/<tr><td>(.*?)<\/td>\s*<td>(.*?)<\/td>\s*<\/tr>/i";
$str = "<table> <tr><td>Eric</td><td>25</td></tr> <tr><td>John</td><td>26</td></tr> </table>";
preg_match_all($pre, $str, $matches);
print_r($matches);
```

$matches结果排序为$matches[0]保存完整模式的所有匹配, $matches[1] 保存第一个子组的所有匹配，以此类推。

## 正则表达式的搜索和替换

正则表达式的搜索与替换在某些方面具有重要用途，比如调整目标字符串的格式，改变目标字符串中匹配字符串的顺序等。

例如我们可以简单的调整字符串的日期格式：

```php
$string = 'April 15, 2014';
$pattern = '/(\w+) (\d+), (\d+)/i';
$replacement = '$3, ${1} $2';
echo preg_replace($pattern, $replacement, $string); //结果为：2014, April 15
```

其中${1}与$1的写法是等效的，表示第一个匹配的字串，$2代表第二个匹配的。

通过复杂的模式，我们可以更加精确的替换目标字符串的内容。

```php
$patterns = array ('/(19|20)(\d{2})-(\d{1,2})-(\d{1,2})/',
                   '/^\s*{(\w+)}\s*=/');
$replace = array ('\3/\4/\1\2', '$\1 =');//\3等效于$3,\4等效于$4，依次类推
echo preg_replace($patterns, $replace, '{startDate} = 1999-5-27'); //结果为：$startDate = 5/27/1999
```

//详细解释下结果：(19|20)表示取19或者20中任意一个数字，(\d{2})表示两个数字，(\d{1,2})表示1个或2个数字，(\d{1,2})表示1个或2个数字。^\s*{(\w+)\s*=}表示以任意空格开头的，并且包含在{}中的字符，并且以任意空格结尾的，最后有个=号的。
用正则替换来去掉多余的空格与字符：

```php
$str = 'one     two';
$str = preg_replace('/\s+/', ' ', $str);
echo $str; // 结果改变为'one two'
```

## PCRE 函数

| 函数                                                         | 描述                                           |
| ------------------------------------------------------------ | ---------------------------------------------- |
| [preg_filter](http://www.runoob.com/php/php-preg_filter.html) | 执行一个正则表达式搜索和替换                   |
| [preg_grep](http://www.runoob.com/php/php-preg_grep.html)    | 返回匹配模式的数组条目                         |
| [preg_last_error](http://www.runoob.com/php/php-preg_last_error.html) | 返回最后一个PCRE正则执行产生的错误代码         |
| [preg_match_all](http://www.runoob.com/php/php-preg_match_all.html) | 执行一个全局正则表达式匹配                     |
| [preg_match](http://www.runoob.com/php/php-preg_match.html)  | 执行一个正则表达式匹配                         |
| [preg_quote](http://www.runoob.com/php/php-preg_quote.html)  | 转义正则表达式字符                             |
| [preg_replace_callback_array](http://www.runoob.com/php/php-preg_replace_callback_array.html) | 执行一个正则表达式搜索并且使用一个回调进行替换 |
| [preg_replace_callback](http://www.runoob.com/php/php-preg_replace_callback.html) | 执行一个正则表达式搜索并且使用一个回调进行替换 |
| [preg_replace](http://www.runoob.com/php/php-preg_replace.html) | 执行一个正则表达式的搜索和替换                 |
| [preg_split](http://www.runoob.com/php/php-preg_split.html)  | 通过一个正则表达式分隔字符串                   |

## PREG 常量

| 常量                           | 描述                                                         | 自从  |
| ------------------------------ | ------------------------------------------------------------ | ----- |
| **PREG_PATTERN_ORDER**         | 结果按照"规则"排序，仅用于preg_match_all()， 即$matches[0]是完整规则的匹配结果， $matches[1]是第一个子组匹配的结果，等等。 | since |
| **PREG_SET_ORDER**             | 结果按照"集合"排序，仅用于preg_match_all()， 即$matches[0]保存第一次匹配结果的所有结果(包含子组)信息, $matches[1]保存第二次的结果信息，等等。 |       |
| **PREG_OFFSET_CAPTURE**        | 查看**PREG_SPLIT_OFFSET_CAPTURE**的描述。                    | 4.3.0 |
| **PREG_SPLIT_NO_EMPTY**        | 这个标记告诉 preg_split() 进返回非空部分。                   |       |
| **PREG_SPLIT_DELIM_CAPTURE**   | 这个标记告诉 preg_split() 同时捕获括号表达式匹配到的内容。   | 4.0.5 |
| **PREG_SPLIT_OFFSET_CAPTURE**  | 如果设置了这个标记，每次出现的匹配子串的偏移量也会被返回。注意，这会改变返回数组中的值， 每个元素都是由匹配子串作为第0个元素，它相对目标字符串的偏移量作为第1个元素的数组。这个 标记只能用于 preg_split()。 | 4.3.0 |
| **PREG_NO_ERROR**              | 没有匹配错误时调用 preg_last_error() 返回。                  | 5.2.0 |
| **PREG_INTERNAL_ERROR**        | 如果有PCRE内部错误时调用 preg_last_error() 返回。            | 5.2.0 |
| **PREG_BACKTRACK_LIMIT_ERROR** | 如果调用回溯限制超出，调用preg_last_error()时返回。          | 5.2.0 |
| **PREG_RECURSION_LIMIT_ERROR** | 如果递归限制超出，调用preg_last_error()时返回。              | 5.2.0 |
| **PREG_BAD_UTF8_ERROR**        | 如果最后一个错误时由于异常的utf-8数据(仅在运行在 UTF-8 模式正则表达式下可用)。 导致的，调用preg_last_error()返回。 | 5.2.0 |
| **PREG_BAD_UTF8_OFFSET_ERROR** | 如果偏移量与合法的urf-8代码不匹配(仅在运行在 UTF-8 模式正则表达式下可用)。 调用preg_last_error()返回。 | 5.3.0 |
| **PCRE_VERSION**               | PCRE版本号和发布日期(比如： "*7.0 18-Dec-2006*")。           | 5.2.4 |

## ETC

正则表达式是一种描述字符串结果的语法规则，是一个特定的格式化模式，可以匹配、替换、截取匹配的字符串。常用的语言基本上都有正则表达式，如JavaScript、java等。其实，只有了解一种语言的正则使用，其他语言的正则使用起来，就相对简单些。好了，开始写正则了。

正则表达式在匹配字符串时，遵循以下2个基本原则：
1.最左原则：正则表达式总是从目标字符串的最左侧开始，依次匹配，直到匹配到符合表达式要求的部分，或直到匹配目标字符串的结束。
2.最长原则：对于匹配到的目标字符串，正则表达式总是会匹配到符合正则表达式要求的最长的部分；即贪婪模式

那怎么开始呢，首先从分隔符开始写起，常用包括 / ; #；~，用于表明一串正则的开始。如：‘/a.*a/’。当表达式有过多的转义字符时，建议优先使用#，如url;

```php
$str = 'http://baidu.com';

$pattern = '/http:\/\/.*com/';//需要转义/

preg_match($pattern,$str,$match);

var_dump( $match);
```

```php
$str = 'http://baidu.com';

$pattern = '#http://.*com#';//不需要转义/

preg_match($pattern,$str,$match);

var_dump( $match);
```

知道开始和结尾的写法了，接下来就是中间的判断了。正则表达式是自左向右的顺序使用原子和元字符进行拼接。比如'`<b>zxcv</b>`',进行匹配时，‘`/<b>.*<\/b>/`’,其中.*代表zxcv 。那么通用原子和元字符有哪些呢?

- `\d`	匹配一个数字字符。等价于 `[0-9]`。
- `\D`	匹配一个非数字字符。等价于 `[^0-9]`。
- `\f`	匹配一个换页符。等价于 `\x0c` 和 `\cL`。
- `\n`	匹配一个换行符。等价于 `\x0a` 和 `\cJ`。
- `\r`	匹配一个回车符。等价于 `\x0d` 和 `\cM`。
- `\s`	匹配任何空白字符，包括空格、制表符、换页符等等。等价于 `[ \f\n\r\t\v]`。
- `\S`	匹配任何非空白字符。等价于 `[^ \f\n\r\t\v]`。
- `\t`	匹配一个制表符。等价于 `\x09` 和 `\cI`。
- `\v`	匹配一个垂直制表符。等价于 `\x0b` 和 `\cK`。
- `\w`	匹配包括下划线的任何单词字符。等价于’`[A-Za-z0-9_]`’。
- `\W`	匹配任何非单词字符。等价于 ‘`[^A-Za-z0-9_]`’。
- `\xn`	匹配 n，其中 n 为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，’`\x41`’ 匹配 “`A`”。’`\x041`’ 则等价于 ‘`\x04`’ `&` “`1`”。正则表达式中可以使用 ASCII 编码。	
- `\nm`	标识一个八进制转义值或一个向后引用。如果 `\nm` 之前至少有 nm 个获得子表达式，则 nm 为向后引用。如果 \nm 之前至少有 n 个获取，则 n 为一个后跟文字 m 的向后引用。如果前面的条件都不满足，若 n 和 m 均为八进制数字 `(0-7`)，则 • `\nm` 将匹配八进制转义值 `nm`。	
- `\nml`	如果 n 为八进制数字 `(0-3)`，且 m 和 l 均为八进制数字 `(0-7)`，则匹配八进制转义值 `nml`。	
- `\un`	十六进制数字表示的 Unicode 字符。例如， \u00A9 匹配版权符号(?)。
- `.`	 匹配除 “`\n`” 之外的任何单个字符	
- `^` 匹配输入字符串的开始位置。在字符域`[]`中表示取反，如'`[^\w]`'等于'`\w`';而`^\w`表示以单词字符开头。
- `$` 匹配输入字符串的结束位置。例'`\w$`'表示以单词字符结尾。
- `？` 匹配前面的子表达式零次或一次 等价于 `{0,1}`，例如，"`do(es)?`" 可以匹配 "`do`" 或 "`does`"。
- `*` 匹配前面的子表达式零次或多次，等价于{0,}。例如，`zo*` 能匹配 "`z`" 、 "`zo`"、'`zoo`'。
- `+` 匹配前面的子表达式一次或多次，等价于{1,}例如，'zo+' 能匹配 "zo" 以及 "zoo"。
- `{n}` n 为非负整数,匹配确定的 n 次。例如，'`o{2}`' 不能匹配 "Bob" 或‘Booob’，但是能匹配 "food" 中的两个 o。
- `{n,}` n 为非负整数。至少匹配n 次。例如，'`o{2,}`' 不能匹配 "Bob" 中的 'o'，但能匹配 "`foooood`" 中的所有 o。'`o{1,}`' 等价于 '`o+`'。'`o{0,}`' 则等价于 '`o*`'。
- `{n,m}` m 和 n 均为非负整数，其中`n <= m`。最少匹配 n 次且最多匹配 m 次。例如，"`o{1,3}`" 将匹配 "`fooooood`" 中的前三个 o。'`o{0,1}`' 等价于 '`o?`'。请注意在逗号和两个数之间不能有空格。
- `[]` 字符集合(字符域)。匹配所包含的任意一个字符。例如， '[abc]' 可以匹配 "`plain`" 中的 'a'。
- `()` 匹配 ()内的内容 并获取这一匹配。搭配`\n`(n为大于1的整数)，‘`http://baidu.com`’若表达式：‘`（\w+） (:)\/\/.*\1`’则匹配‘`http://baidu.comhttp`’,`\1`表示`http`。
- `(?:)` 匹 配 但不获取匹配结果，不进行存储供以后使用。这在使用 "或" 字符 `(|)` 来组合一个模式的各个部分是很有用。例如， '`industr(?:y|ies)` 就是一个比 '`industry|industries`' 更简略的表达式。上面表达式若改为'`（?:\w+）(:)\/\/.*\1`'，则\1表示为：
- `|` x|y,匹配 x 或 y。例如，'`z|food`' 能匹配 "z" 或 "`food`"。'`(z|f)ood`' 则匹配 "`zood`" 或 "`food`"。
- `[-]` 字符范围。匹配指定范围内的任意字符。例如，'[a-z]' 可以匹配 'a' 到 'z' 范围内的任意小写字母字符。
- `(?=pattern)`	正 向预查，在任何匹配 pattern 的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹 配不需要获取供以后使用。例如，'`Windows (?=95|98|NT|2000)`' 能匹配 "Windows 2000" 中的 "Windows" ，但不能匹配 "Windows 3.1" 中的 "Windows"。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹 配的搜索，而不是从包含预查的字符之后开始。
- `(?!pattern)`	负 向预查，在任何不匹配 pattern 的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不 需要获取供以后使用。例如'`Windows (?!95|98|NT|2000)`' 能匹配 "Windows 3.1" 中的 "Windows"，但不能匹配 "Windows 2000" 中的 "Windows"。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜 索，而不是从包含预查的字符之后开始

有时候最后定界符会有一个字母，如‘`/as.*/i`’,那这个i又是什么呢，这就是模式修正符；

- `i`	表示在和模式进行匹配进不区分大小写
- `m`	将模式视为多行，使用`^`和`$`表示任何一行都可以以正则表达式开始或结束
- `s`	如果没有使用这个模式修正符号，元字符中的"."默认不能表示换行符号,将字符串视为单行
- `x`	表示模式中的空白忽略不计
- `e`	正则表达式必须使用在`preg_replace`替换字符串的函数中时才可以使用(讲这个函数时再说)
- `A`	以模式字符串开头，相当于元字符`^`
- `Z`	以模式字符串结尾，相当于元字符`$`
- `U`	正则表达式的特点：就是比较“贪婪”，使用该模式修正符可以取消贪婪模式

例：

```php
$str = 'asddadsdasd';

        $pattern = '/a.*d/';

        preg_match($pattern,$str,$match);

        var_dump($match) ;//asddadsdasd;

       $str = 'asddadsdasd';                                  

        $pattern = '/a.*d/U';//$pattern = '/a.*?d/';

        preg_match($pattern,$str,$match);

        var_dump($match) ;//asd
```

php常用正则函数;

匹配：`preg_match()` 与 `preg_match_all()`

1 `preg_match($pattern,$subject,[array &$matches])`
2 `preg_match_all($pattern,$subject,array &$matches)`

1只会匹配一次，2会把所有符合的字符串都匹配出来，并且放置到matches数组中，而且这两个函数都有一个整形的返回 值。1是一维数组，2是二维数组

替换：`preg_replace()`

`mixed preg_replace ( mixed $pattern , mixed $replacement , mixed $subject [, int $limit = -1 [, int &$count ]] )`
搜索subject中匹配pattern的部分， 以replacement进行替换。

