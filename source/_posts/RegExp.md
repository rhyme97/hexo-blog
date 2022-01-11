---
title: 学习：正则表达式
date: 2020-08-19 10:59:28
tags:
categories:
    - 正则表达式
descriptions:
---

## 什么是正则

正则表达式（Regular Expression 简写regex）是一个很强大的模式语言，使用它我们能够解决很多很棘手的问题，有时候使用字符串查找来解决这类问题不是很方便，所以这个时候正则表达式就能帮我们很大的忙。

完整的正则表达式由两种字符构成。特殊字符(specialcharacters,比如*) 称为“元字符”(metacharacters),其他为“文字”(literal),或者是普通文本字符(normaltext characters).

## 正则组成

### 表达式部分

#### 普通字符

普通字符包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。

``` js
//匹配字母‘abc’
let re =/abc/
let res = re.test('asdfabcs')
console.log(res)//true
```

#### 特殊字符

所谓特殊字符，就是一些有特殊含义的字符。如果要查找的字符串是一个特殊字符，则需要对其进行转义，即在其前加一个 `\`

下表列出了正则表达式中的一些特殊字符：

| 特别字符 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| $        | 匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用 \$。 |
| .        | 匹配除换行符 \n 之外的任何单字符。                           |
| \        | 将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"，而 '\(' 则匹配 "("。 |
| ^        | 匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \^。 |
| \n       | 匹配一个换行符。                                             |

``` js
let str = 'ab^sc$c';
//匹配sc结尾而不是匹配字符串'sc$'
let re1 = /sc$/
console.log(str.match(re1))//null
//匹配sc开始而不是匹配字符串'^sc'
let re2 = /^sc/
console.log(str.match(re2))//null
//转义
let re3 = /\^sc/
console.log(str.match(re3))//['^sc']
```

### 表达式之外

#### 修饰符

标记也称为修饰符，正则表达式的标记用于指定额外的匹配策略。

标记不写在正则表达式里，标记位于表达式之外，格式如下：

```
/pattern/flags
```

下表列出了正则表达式常用的修饰符：

| 修饰符 | 含义                                   | 描述                                                         |
| :----- | :------------------------------------- | :----------------------------------------------------------- |
| i      | ignore - 不区分大小写                  | 将匹配设置为不区分大小写，搜索时不区分大小写: A 和 a 没有区别。 |
| g      | global - 全局匹配                      | 查找所有的匹配项。                                           |
| m      | more - 多行匹配                        | 使边界字符 **^** 和 **$** 匹配每一行的开头和结尾，记住是多行，而不是整个字符串的开头和结尾。 |
| s      | 特殊字符圆点 **.** 中包含换行符 **\n** | 默认情况下的圆点 **.** 是 匹配除换行符 **\n** 之外的任何字符，加上 **s** 修饰符之后, **.** 中包含换行符 \n。 |

示例1

``` js
let s = 'xxxxabxxabxx';
//不加全局的匹配
let re = /ab/;
s.match(re) //["ab"]
//使用全局的作用
let re = /ab/g;
s.match(re) //["ab", "ab"];
//不区分大小写
let re = /Ab/i
s.match(re) // ['ab']
```

示例2：

> m 修饰符可以使 **^** 和 **$** 匹配一段文本中每行的开始和结束位置。g 只匹配第一行，添加 m 之后实现多行。![](./images/BC3E6D8A-21D2-44F8-A1AE-D90C4939D37A.jpg)

示例3

> 默认情况下的圆点 **.** 是 匹配除换行符 **\n** 之外的任何字符，加上 s 之后, **.** 中包含换行符 **\n**。![](images\5CDFC964-F0C4-4ADE-80F3-17FB4748DE14.jpg)

## 在javascript中使用

>  正则表达式是一种通用的工具，在 [Java](http://c.biancheng.net/java/)Script、[PHP](http://c.biancheng.net/php/)、Java、[Python](http://c.biancheng.net/python/)、[C++](http://c.biancheng.net/cplus/) 等几乎所有的编程语言中都能使用；但是，不同编程语言对正则表达式语法的支持不尽相同，有的编程语言支持所有的语法，有的仅支持一个子集。本节讲到的正则表达式语法适用于 JavaScript。

> 本文档将通过js语言展示正则表达示例

### 创建正则表达式

你可以使用以下两种方法构建一个正则表达式：

1.使用一个正则表达式字面量，其由包含在斜杠之间的模式组成，如下所示：

```js
var re = /ab+c/;
```

2.或者调用`RegExp`对象的构造函数，如下所示：

```js
var re = new RegExp("ab+c");
```

从 ECMAScript 6 开始，当第一个参数为正则表达式而第二个标志参数存在时，`new RegExp(/ab+c/, 'i')` 不再抛出 [`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError) （`"从另一个RegExp构造一个RegExp时无法提供标志"`）的异常，取而代之，将使用这些参数创建一个新的正则表达式。

```javascript
new RegExp('ab+c', 'i');
```

### 使用正则表达式

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`exec`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) | 一个在字符串中执行查找匹配的RegExp方法，它返回一个数组（未匹配到则返回 null）。(会更新正则表达式对象的 [`lastIndex`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/lastIndex) 属性) |
| [`test`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) | 一个在字符串中测试是否匹配的RegExp方法，它返回 true 或 false。 |
| [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match) | 一个在字符串中执行查找匹配的String方法，它返回一个数组，在未匹配到时会返回 null。 |
| [`matchAll`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll) | 一个在字符串中执行查找所有匹配的String方法，它返回一个迭代器（iterator）。 |
| [`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) | 一个在字符串中测试匹配的String方法，它返回匹配到的位置索引，或者在失败时返回-1。 |
| [`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace) | 一个在字符串中执行查找匹配的String方法，并且使用替换字符串替换掉匹配到的子字符串。 |
| [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) | 一个使用正则表达式或者一个固定字符串分隔一个字符串，并将分隔后的子字符串存储到数组中的 `String` 方法。 |

##  正则语法

主体就是对元字符的使用，下表包含了元字符的完整列表以及它们在正则表达式上下文中的行为：

| 字符         | 描述                                                         |
| :----------- | :----------------------------------------------------------- |
| \            | 将下一个字符标记为一个特殊字符、或一个原义字符、或一个 向后引用、或一个八进制转义符。例如，'n' 匹配字符 "n"。'\n' 匹配一个换行符。序列 '\\' 匹配 "\" 而 "\(" 则匹配 "("。 |
| ^            | 匹配输入字符串的开始位置。如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 '\n' 或 '\r' 之后的位置。 |
| $            | 匹配输入字符串的结束位置。如果设置了RegExp 对象的 Multiline 属性，$ 也匹配 '\n' 或 '\r' 之前的位置。 |
| *            | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。 |
| +            | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。 |
| ?            | 匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 或 "does" 。? 等价于 {0,1}。 |
| {n}          | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。 |
| {n,}         | n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。 |
| {n,m}        | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和两个数之间不能有空格。 |
| ?            | 当该字符紧跟在任何一个其他限制符 (*, +, ?, {n}, {n,}, {n,m}) 后面时，匹配模式是非贪婪的。非贪婪模式尽可能少的匹配所搜索的字符串，而默认的贪婪模式则尽可能多的匹配所搜索的字符串。例如，对于字符串 "oooo"，'o+?' 将匹配单个 "o"，而 'o+' 将匹配所有 'o'。 |
| .            | 匹配除换行符（\n、\r）之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用像"**(.\|\n)**"的模式。 |
| (pattern)    | 匹配 pattern 并获取这一匹配。所获取的匹配可以从产生的 Matches 集合得到，在VBScript 中使用 SubMatches 集合，在JScript 中则使用 $0…$9 属性。要匹配圆括号字符，请使用 '\(' 或 '\)'。 |
| (?:pattern)  | 匹配 pattern 但不获取匹配结果，也就是说这是一个非获取匹配，不进行存储供以后使用。这在使用 "或" 字符 (\|) 来组合一个模式的各个部分是很有用。例如， 'industr(?:y\|ies) 就是一个比 'industry\|industries' 更简略的表达式。 |
| (?=pattern)  | 正向肯定预查（look ahead positive assert），在任何匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如，"Windows(?=95\|98\|NT\|2000)"能匹配"Windows2000"中的"Windows"，但不能匹配"Windows3.1"中的"Windows"。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| (?!pattern)  | 正向否定预查(negative assert)，在任何不匹配pattern的字符串开始处匹配查找字符串。这是一个非获取匹配，也就是说，该匹配不需要获取供以后使用。例如"Windows(?!95\|98\|NT\|2000)"能匹配"Windows3.1"中的"Windows"，但不能匹配"Windows2000"中的"Windows"。预查不消耗字符，也就是说，在一个匹配发生后，在最后一次匹配之后立即开始下一次匹配的搜索，而不是从包含预查的字符之后开始。 |
| (?<=pattern) | 反向(look behind)肯定预查，与正向肯定预查类似，只是方向相反。例如，"`(?<=95|98|NT|2000)Windows`"能匹配"`2000Windows`"中的"`Windows`"，但不能匹配"`3.1Windows`"中的"`Windows`"。 |
| (?<!pattern) | 反向否定预查，与正向否定预查类似，只是方向相反。例如"`(?<!95|98|NT|2000)Windows`"能匹配"`3.1Windows`"中的"`Windows`"，但不能匹配"`2000Windows`"中的"`Windows`"。 |
| x\|y         | 匹配 x 或 y。例如，'z\|food' 能匹配 "z" 或 "food"。'(z\|f)ood' 则匹配 "zood" 或 "food"。 |
| [xyz]        | 字符集合。匹配所包含的任意一个字符。例如， '[abc]' 可以匹配 "plain" 中的 'a'。 |
| [^xyz]       | 负值字符集合。匹配未包含的任意字符。例如， '[^abc]' 可以匹配 "plain" 中的'p'、'l'、'i'、'n'。 |
| [a-z]        | 字符范围。匹配指定范围内的任意字符。例如，'[a-z]' 可以匹配 'a' 到 'z' 范围内的任意小写字母字符。 |
| [^a-z]       | 负值字符范围。匹配任何不在指定范围内的任意字符。例如，'[^a-z]' 可以匹配任何不在 'a' 到 'z' 范围内的任意字符。 |
| \b           | 匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。 |
| \B           | 匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。 |
| \cx          | 匹配由 x 指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。 |
| \d           | 匹配一个数字字符。等价于 [0-9]。                             |
| \D           | 匹配一个非数字字符。等价于 [^0-9]。                          |
| \f           | 匹配一个换页符。等价于 \x0c 和 \cL。                         |
| \n           | 匹配一个换行符。等价于 \x0a 和 \cJ。                         |
| \r           | 匹配一个回车符。等价于 \x0d 和 \cM。                         |
| \s           | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。 |
| \S           | 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。                  |
| \t           | 匹配一个制表符。等价于 \x09 和 \cI。                         |
| \v           | 匹配一个垂直制表符。等价于 \x0b 和 \cK。                     |
| \w           | 匹配字母、数字、下划线。等价于'[A-Za-z0-9_]'。               |
| \W           | 匹配非字母、数字、下划线。等价于 '[^A-Za-z0-9_]'。           |
| \xn          | 匹配 n，其中 n 为十六进制转义值。十六进制转义值必须为确定的两个数字长。例如，'\x41' 匹配 "A"。'\x041' 则等价于 '\x04' & "1"。正则表达式中可以使用 ASCII 编码。 |
| \num         | 匹配 num，其中 num 是一个正整数。对所获取的匹配的引用。例如，'(.)\1' 匹配两个连续的相同字符。 |
| \n           | 标识一个八进制转义值或一个向后引用。如果 \n 之前至少 n 个获取的子表达式，则 n 为向后引用。否则，如果 n 为八进制数字 (0-7)，则 n 为一个八进制转义值。 |
| \nm          | 标识一个八进制转义值或一个向后引用。如果 \nm 之前至少有 nm 个获得子表达式，则 nm 为向后引用。如果 \nm 之前至少有 n 个获取，则 n 为一个后跟文字 m 的向后引用。如果前面的条件都不满足，若 n 和 m 均为八进制数字 (0-7)，则 \nm 将匹配八进制转义值 nm。 |
| \nml         | 如果 n 为八进制数字 (0-3)，且 m 和 l 均为八进制数字 (0-7)，则匹配八进制转义值 nml。 |
| \un          | 匹配 n，其中 n 是一个用四个十六进制数字表示的 Unicode 字符。例如， \u00A9 匹配版权符号 (?)。 |

在整体列表基础上，我们可以分类的去阐述它们的作用：

### 非打印字符

非打印字符也可以是正则表达式的组成部分。下表列出了表示非打印字符的转义序列：

| 字符 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| \cx  | 匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。 |
| \f   | 匹配一个换页符。等价于 \x0c 和 \cL。                         |
| \n   | 匹配一个换行符。等价于 \x0a 和 \cJ。                         |
| \r   | 匹配一个回车符。等价于 \x0d 和 \cM。                         |
| \s   | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。 |
| \S   | 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。                  |
| \t   | 匹配一个制表符。等价于 \x09 和 \cI。                         |
| \v   | 匹配一个垂直制表符。等价于 \x0b 和 \cK。                     |

```js
//匹配任何空白字符
let re =/\s/
let res = re.test('say hi')
console.log(res)//true
//匹配任何非空白字符
let re =/\S/
let res = re.test(' ')
console.log(res)//false
```

### 限定符

限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 ***** 或 **+** 或 **?** 或 **{n}** 或 **{n,}** 或 **{n,m}** 共6种。

正则表达式的限定符有：

| 字符  | 描述                                                         |
| :---- | :----------------------------------------------------------- |
| *     | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。 |
| +     | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。 |
| ?     | 匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 、 "does" 中的 "does" 、 "doxy" 中的 "do" 。? 等价于 {0,1}。 |
| {n}   | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。 |
| {n,}  | n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。 |
| {n,m} | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和两个数之间不能有空格。 |

``` js
var s = "ggle gogle google gooogle goooogle ";
//如果仅匹配单词 ggle 和 gogle，可以设计：
var r = /go?gle/g;
var a = s.match(r);
//量词?表示前面字符或子表达式为可有可无，等效于：
var r = /go{0,1}gle/g;
//如果匹配所有单词，可以设计：
var r = /go*gle/g;
//匹配包含字符“o”的所有词
var r = /go+gle/g;
//匹配字母o出现三次
var r =/o{3}/g //["ooo", "ooo"]
//匹配字母o出现三次以上的
var r =/o{3,}/g //["ooo", "oooo"]
//匹配字母o出现两到三次
var r =/o{2,3}/g  //["oo", "ooo", "ooo"]
```

### 限定符特性

#### 贪婪

重复类量词都具有贪婪性，在条件允许的前提下，会匹配尽可能多的字符。

- ?、{n} 和 {n,m} 重复类具有弱贪婪性，表现为贪婪的有限性。
- *、+ 和 {n,} 重复类具有强贪婪性，表现为贪婪的无限性。

``` js
//例如，您可能搜索 HTML 文档，以查找在 h1 标签内的内容。HTML 代码如下：
var s = "<h1>abc</h1>";
var r = /<.*>/
var a = s.match(r); //["<h1>abc</h1>"]
```

#### 非贪婪

与贪婪匹配相反，惰性匹配将遵循另一种算法：在满足条件的前提下，尽可能少的匹配字符。定义惰性匹配的方法：在重复类量词后面添加问号?限制词。贪婪匹配体现了最大化匹配原则，惰性匹配则体现最小化匹配原则。

```js
var s = "<html><head><title></title></head><body></body></html>";
var r = /<.*?>/
var a = s.match(r);  //返回单个元素数组["<html>"]
```

### 定位符

定位符使您能够将正则表达式固定到行首或行尾。它们还使您能够创建这样的正则表达式，这些正则表达式出现在一个单词内、在一个单词的开头或者一个单词的结尾。

定位符用来描述字符串或单词的边界，**^** 和 **$** 分别指字符串的开始与结束，**\b** 描述单词的前或后边界，**\B** 表示非单词边界。

正则表达式的定位符有：

| 字符 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| ^    | 匹配输入字符串开始的位置。如果设置了 RegExp 对象的 Multiline 属性，^ 还会与 \n 或 \r 之后的位置匹配。 |
| $    | 匹配输入字符串结尾的位置。如果设置了 RegExp 对象的 Multiline 属性，$ 还会与 \n 或 \r 之前的位置匹配。 |
| \b   | 匹配一个单词边界，即字与空格间的位置。                       |
| \B   | 非单词边界匹配。                                             |

**注意**：不能将限定符与定位符一起使用。由于在紧靠换行或者单词边界的前面或后面不能有一个以上位置，因此不允许诸如 **^\*** 之类的表达式。

若要匹配一行文本开始处的文本，请在正则表达式的开始使用 **^** 字符。不要将 **^** 的这种用法与中括号表达式内的用法混淆。

若要匹配一行文本的结束处的文本，请在正则表达式的结束处使用 **$** 字符。

若要在搜索章节标题时使用定位点，下面的正则表达式匹配一个章节标题，该标题只包含两个尾随数字，并且出现在行首：

```js
/^Chapter [1-9][0-9]{0,1}/
```

真正的章节标题不仅出现行的开始处，而且它还是该行中仅有的文本。它即出现在行首又出现在同一行的结尾。下面的表达式能确保指定的匹配只匹配章节而不匹配交叉引用。通过创建只匹配一行文本的开始和结尾的正则表达式，就可做到这一点。

```js
/^Chapter [1-9][0-9]{0,1}$/
```

匹配单词边界稍有不同，但向正则表达式添加了很重要的能力。单词边界是单词和空格之间的位置。非单词边界是任何其他位置。下面的表达式匹配单词 Chapter 的开头三个字符，因为这三个字符出现在单词边界后面：

```js
/\bCha/
```

**\b** 字符的位置是非常重要的。如果它位于要匹配的字符串的开始，它在单词的开始处查找匹配项。如果它位于字符串的结尾，它在单词的结尾处查找匹配项。例如，下面的表达式匹配单词 Chapter 中的字符串 ter，因为它出现在单词边界的前面：

```js
/ter\b/
```

下面的表达式匹配 Chapter 中的字符串 apt，但不匹配 aptitude 中的字符串 apt：

```js
/\Bapt/
```

字符串 apt 出现在单词 Chapter 中的非单词边界处，但出现在单词 aptitude 中的单词边界处。对于 **\B** 非单词边界运算符，位置并不重要，因为匹配不关心究竟是单词的开头还是结尾。

### 描述字符范围

在正则表达式语法中，放括号表示字符范围。在方括号中可以包含多个字符，表示匹配其中任意一个字符。如果多个字符的编码顺序是连续的，可以仅指定开头和结尾字符，省略中间字符，仅使用连字符`~``^`

- [abc]：查找方括号内任意一个字符。
- [^abc]：查找不在方括号内的字符。
- [0-9]：查找从 0 至 9 范围内的数字，即查找数字。
- [a-z]：查找从小写 a 到小写 z 范围内的字符，即查找小写字母。
- [A-Z]：查找从大写 A 到大写 Z 范围内的字符，即查找大写字母。
- [A-z]：查找从大写 A 到小写 z 范围内的字符，即所有大小写的字母。

示例

```js
var r = /[a-zA-Z0-9]/g;  //如果匹配任意大小写字母和数字：

var s = "abcdez";  //字符串直接量
var r = /[abce-z]/g;  //字符a、b、c，以及从e~z之间的任意字符
var a = s.match(r);  //返回数组["a","b","c","e","z"]

var s = "abc4 abd6 abe3 abf1 abg7";  //字符串直接量
var r = /ab[c-g][1-7]/g;  //前两个字符为ab，第三个字符为从c到g，第四个字符为1~7的任意数字
var a = s.match(r);  //返回数组["abc4","abd6","abe3","abf1","abg7"]

//使用反义字符范围可以匹配很多无法直接描述的字符，达到以少应多的目的。
//在这个正则表达式中，将会匹配除了数字以外任意的字符。
var r = /[^0123456789]/g;
```

> 注意：在中括号内不要有空格，否则会误解为还要匹配空格。

### 分组选择

用圆括号 **()** 将所有选择项括起来，相邻的选择项之间用 **|** 分隔。

**()** 表示捕获分组，**()** 会把每个分组里的匹配的值保存起来， 多个匹配值可以通过数字 n 来查看(**n** 是一个数字，表示第 n 个捕获组的内容)。

#### 分组捕获

``` js
//匹配130/131/132/155/156/185/186/145/176开头的手机号
var str="13055559999";
let re =/^(130|131|132|155|156|185|186|145|176)\d{8}$/
var n1=re.exec(str)
//["13055559999", "130", index: 0, input: "13055559999", groups: undefined]
```

#### 非捕获

但用圆括号会有一个副作用，使相关的匹配会被缓存，此时可用 **?:** 放在第一个选项前来消除这种副作用。还有两个非捕获元是 **?=** 和 **?!**

``` js
//匹配130/131/132/155/156/185/186/145/176开头的手机号
var str="13055559999";
let re =/^(?:130|131|132|155|156|185|186|145|176)\d{8}$/
var n1=re.exec(str)
//["13055559999", index: 0, input: "13055559999", groups: undefined]
```



### 反向引用

所谓反向引用其实就是 \1、\2 ... ... 这些，他的作用相当于引用“一组规则”；那么什么叫一组规则呢？举个例子：

``` 
/([a-z]\d)-\d(123)\1/
```

这里有两组小括号（），每一组小括号就是所谓的一组；因此这个正则中的\1指的便是

```html
([a-z]\d)
```

反向引用的最简单的、最有用的应用之一，是提供查找文本中两个相同的相邻单词的匹配项的能力。以下面的句子为例：

``` js
let str = 'i i am a coder, i like like coding and my my friend like sports !'
console.log(str.match(/([a-z]+)\s+\1/g))
// ["i i", "like like", "my my"]
```

当（[a-z]+）匹配了内容后，后面的\1 要匹配的是与第一组规则匹配到的内容一样的内容。

### 环视（零宽断言）

![img](images\17332c80a25b201f.jpg)

当然，仅看这张表是很懵的。我来解释下，零宽断言其实也就是对要匹配项的左右做一定的限制，对应的就是这4种情况。

**exp1(?=exp2)**：查找 exp2 前面的 exp1。![img](images\reg-111.jpg)

**(?<=exp2)exp1**：查找 exp2 后面的 exp1。![img](images\reg-222.jpg)

**exp1(?!exp2)**：查找后面不是 exp2 的 exp1。![img](images\reg-333.jpg)

**(?<!=exp2)exp1**：查找前面不是 exp2 的 exp1。![img](images\reg-444.jpg)



## 补充

### 运算符优先级

正则表达式从左到右进行计算，并遵循优先级顺序，这与算术表达式非常类似。

相同优先级的从左到右进行运算，不同优先级的运算先高后低。下表从最高到最低说明了各种正则表达式运算符的优先级顺序：

| 运算符                      | 描述                                                         |
| :-------------------------- | :----------------------------------------------------------- |
| \                           | 转义符                                                       |
| (), (?:), (?=), []          | 圆括号和方括号                                               |
| *, +, ?, {n}, {n,}, {n,m}   | 限定符                                                       |
| ^, $, \任何元字符、任何字符 | 定位点和序列（即：位置和顺序）                               |
| \|                          | 替换，"或"操作 字符具有高于替换运算符的优先级，使得"m\|food"匹配"m"或"food"。若要匹配"mood"或"food"，请使用括号创建子表达式，从而产生"(m\|f)ood"。 |



### 正则匹配原理

正则表达是的执行，是由正则表达引擎编译执行的，正则引擎主要可以分为基本不同的两大类：一种是DFA（确定型有穷自动机），另一种是NFA（不确定型有穷自动机）。

这里需要和大家解释下何为`确定型`、`有穷`、`自动机`这几个名词：

1. **确定型与非确定型**：假设有一个字符串（text=abc）需要匹配，在没有编写正则表达式的前提下，就直接**可以确定字符匹配顺序的就是确定型**，不能确定字符匹配顺序的则为非确定型。
2. **有穷**：有穷即表示有限的意思，这里表示有限次数内能得到结果。
3. **自动机**：自动机便是自动完成，在我们设置好匹配规则后由引擎自动完成，不需要人为干预！

根据上面的解释我们可得知**DFA引擎 和 NFA引擎 的区别就在于**：在没有编写正则表达式的前提下，是否能确定字符执行顺序！

DFA引擎的一些特点：

![](images\v2-b6a6db571846437459a8c4185c2a5793_b.gif)

1. **文本主导**：按照文本的顺序执行，这也就能说明为什么DFA引擎是确定型(deterministic)了，稳定！
2. **记录当前有效的所有可能**：我们看到当执行到`(d|b)`时，同时比较表达式中的`d`和`b`，所以会需要更多的内存。
3. **每个字符只检查一次**：这提高了执行效率，而且速度与正则表达式无关。
4. **不能使用反向引用等功能**：因为每个字符只检查一次，文本零宽度（位置）只记录当前比较值，所以不能使用反向引用、环视等一些功能！

NFA引擎的一些特点：

![](images\v2-dc5e751552ff0fb9e1c82e976e623cc0_b.gif)

1. **表达式主导**：按照表达式的一部分执行，如果不匹配换其他部分继续匹配，直到表达式匹配完成。
2. **会记录某个位置**：我们看到当执行到`(d|b)`时，NFA引擎会记录字符的位置（零宽度），然后选择其中一个先匹配。
3. **单个字符可能检查多次**：我们看到当执行到`(d|b)`时，比较`d`后发现不匹配，于是NFA引擎换表达式的另一个分支`b`，同时文本位置**回退**，重新匹配字符'b'。这也是NFA引擎是非确定型的原因，同时带来另一个问题效率可能没有DFA引擎高。
4. **可实现反向引用等功能**：因为具有**回退**这一步，所以可以很容易的实现反向引用、环视等一些功能！

两种引擎的区别:

![](images\v2-b71db906bee758c7aebbaa0151bc2a7d_720w.jpg)

### 回溯

作为绝大多数编程语言都选择的引擎——NFA (非确定型有穷自动机) 引擎，我们当然要再详细了解一下它的精髓——**回溯**。

对于下面这个表达式，相信大家很清楚它的意图，

``` js
/ab{1,3}c/
```

![1597764428642](images\1597764428642.png)

1~2步应该都好理解，但是为什么在第3步开始，虽然已经文本中已经有一个b匹配了b{1,3}，后面还会拉着字母c跟b{1,3}做比较呢？这个就是我们下面将要提到的正则的贪婪特性，也就是说b{1,3}会竭尽所能的匹配最多的字符。在这个地方我们先知道它一直要匹配到撞上南墙为止。 在这种情况下，第3步发生不匹配之后，整个匹配流程并没有走完，而是像栈一样，将字符c吐出来，然后去用正则表达式中的c去和文本中的c进行匹配。这样就发生了一次回溯。

如果将正则换为

``` js
/ab{1,3}?c/
```

则匹配过程变成了下面这样（橙色为匹配，黄色为不匹配），

![1597764806806](images\1597764806806.png)

### 独占

> ECMAScript不支持

如果在以上重复限定词后加上一个加号（+），则会开启**独占模式**。同贪婪模式一样，独占模式一样会匹配最长。不过在独占模式下，正则表达式尽可能长地去匹配字符串，一旦匹配不成功就会结束匹配而不会回溯。我们以下面的表达式为例，

```js
/ab{1,3}+bc/
```

如果我们用文本"abbc"去匹配上面的表达式，匹配的过程如下图所示（橙色为匹配，黄色为不匹配），



![1597765068523](images\1597765068523.png)

可以发现，在第2和第3步，b{1,3}+会将文本中的2个字母b都匹配上，结果文本中只剩下一个字母c。那么在第4步时，正则中的b和文本中的c进行匹配，当无法匹配时，并不进行回溯，这时候整个文本就无法和正则表达式发生匹配。如果将正则表达式中的加号（+）去掉，那么这个文本整体就是匹配的了。

把以上集中种模式的表达式列出如下，

| **贪婪** | **懒惰** | **独占** |
| -------- | -------- | -------- |
| X?       | X??      | X?+      |
| X*       | X*?      | X*+      |
| X+       | X+?      | X++      |
| X{n}     | X{n}?    | X{n}+    |
| X{n,}    | X{n,}?   | X{n,}+   |
| X{n,m}   | X{n,m}?  | X{n,m}+  |








