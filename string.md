# 字符串
## 概述
### 定义
字符串就是零个或多个排在一起的字符，放在单引号或双引号之中。
```
'abc'
"abc"
```
单引号字符串的内部，可以使用双引号，双引号字符串的内部，也可以使用单引号。
```
'key = "value"'
"It's a long journey"
```
上面俩个都是合法的字符串。

如果要在单引号字符串的内部，使用单引号，就必须在内部的单引号前面加上反斜杠，用来转义。双引号字符串内部使用双引号，也是如此。
```
console.log('Did she say \'hello\'?');   // Did she say 'hello'?
console.log("Did she say \"hello\"?");   // Did she say "hello"?
```
由于 HTML 语言的属性值使用双引号，所以很多项目约定 JavaScript 语言的字符串只使用单引号。当然，只使用双引号也完全可以。重要的是坚持使用一种风格，不要一会使用单引号表示字符串，一会又使用双引号表示。

字符串默认只能写在一行内，分成多行将会报错。
```
var str = 'a
        b';   // '' string literal contains an unescaped line break
```
上面代码将一个字符串分成三行，JavaScript 就会报错。

如果长字符串必须分成多行，可以在每一行的尾部使用反斜杠。
```
var longString = 'Long \
long \
long \
string';
console.log(longString);   // Long long long string
```
上面代码表示，加了反斜杠以后，原来写在一行的字符串，可以分成多行书写。但是，输出的时候还是单行，效果与写在同一行完全一样。注意，反斜杠的后面必须是换行符，而不能有其他字符，比如空格，否则会报错。

连接运算符 ( `+` ) 可以连接多个单行字符串，将长字符串拆成多行书写，输出的时候也是单行。
```
var longString = 'Long ' +
    'long ' +
    'long ' +
    'string';
console.log(longString);   // Long long long string
```
### 转义
反斜杠 ( \ ) 在字符串内有特殊含义，用来表示一些特殊字符，所以又称为转义符。
需要用反斜杠转义的特殊字符，主要有下面这些。
+ \0 : null
+ \b : 后退键
+ \f : 换页符
+ \n : 换行符
+ \r : 回车键
+ \t : 制表符
+ \v : 垂直制表符
+ \' : 单引号
+ \" : 双引号
+ \\ : 反斜杠
上面这些字符前面加上反斜杠，都表示特殊含义。
```
console.log('hello \nworld');   // hello
                                // world
```
上面代码中，`\n` 表示换行，输出的时候就分成了俩行。
### 字符串与数组
字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符，位置编号从0开始。
```
var s = 'hello';
console.log(s[0]);   // h
console.log(s[1]);   // e
console.log(s[2]);   // l
```
但是，字符串与数组的相似性仅此而已。实际上，无法改变字符串之中的单个字符。
```
var s = 'hello';
s[0] = 'e';
console.log(s[0]);   // h
```
上面代码表示，无法对字符串内部做出更改。
### length 属性
`length` 属性返回字符串的长度，该属性也是无法改变的。
```
var s = 'hello';
console.log(s.length);   // 5
```
