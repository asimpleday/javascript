# JSON 对象
## JSON 格式
JSON 格式(JavaScript Object Notation 的缩写)，是一种用于数据交换的数据格式，2001年由 Douglas Crockford 提出，目的是取代繁琐的 XML 格式。

相比较 XML 格式，JSON 格式有俩个显著的优点: 书写简单，一目了然；符合 JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码。所以，JSON 迅速被接受，已经成为各大网站交换数据的标准格式，并被写入标准。

每个 JSON 对象就是一个值，可能是一个数组或对象，也可能是一个原始类型的值。总之，只能是一个值，不能是俩个或更多的值。

JSON 对值的类型和格式有严格的规定。
+ 复合类型的值，只能是数组或对象，不能是函数、正则表达式对象、日期对象。
+ 原始类型的值只能是4种:字符串、数值(必须以十进制表示)、布尔值、null(不能是NAN、Infinity、-Infinity、undefined)。
+ 字符串必须以双引号表示，不能是单引号。
+ 对象的键名必须放在双引号里面。
+ 数组或对象最后一个成员的后面，不能加逗号。

以下都是合法的JSON。
```
["one","two","three"]

{"one": 1, "two": 2, "three": 3}

{"names": ["张三", "李四"]}

[{"name": "张三"}, {"name": "李四"}]
```
以下都是不合法的 JSON。
```

{name: "张三", "age": 24}   // 属性名必须使用双引号

[32, 64, 128, 0xFFF]   // 不能使用十六进制值

{"name": "张三", "age": undefined}   // 不能使用 undefined

{"name": "张三"   
"birthday":  new Date('Fri, 26 Aug 2011 07:13:10 GMT'),
"getName": function(){
		return this.name;
	}
}   // 属性值不能使用函数和日期对象
```
注意，`null`、空数组、空对象都是合法的 JSON 值。
## JSON 对象
`JSON` 对象是 javascript 的原生对象，用来处理 JSON 格式数据。它有俩个静态方法: `JSON.stringify()`、`JSON.parse()`。
### JSON.stringify()
`JSON.stringify()`方法用于将一个值转为 JSON 字符串。该字符串符合 JSON 格式，并且可以被 `JSON.parse()`方法还原。
```
JSON.stringify('abc')   // ""abc""

JSON.stringify(1)   // "1"

JSON.stringify(false)   // "false"

JSON.stringify([])   // "[]"

JSON.stringify({})   // "{}"

JSON.stringify([1, "false", false])   // '[1, "false", false]'

JSON.stringify({name: "张三"})   // {"name": "张三"}
```
上面代码将各种类型的值，转成 JSON 字符串。
注意: 对于原始类型的字符串，转换结果会带双引号。
```
console.log(JSON.stringify('abc') === "abc");   // false

console.log(JSON.stringify('abc') === "\"abc\"");   // true
```
上面代码中，字符串 `abc`，被转成了 `"\"abc\""`。这是因为，将来还原的时候，内层双引号可以让 JavaScript 引擎知道，这是一个字符串，而不是其他类型的值。
```
JSON.stringify(false)   // "false"

JSON.stringify("false")   // "\"false\""
```
上面代码中，如果不是内层的双引号，将来还原的时候，引擎就无法知道原始值是布尔值还是字符串。  

如果对象的属性是 `undefined`、函数、XML 对象，该属性会被 `JSON.stringify()` 过滤。
```
var str = {
    a: undefined,
    b: function () {
        alert(1);
    }
};
console.log(JSON.stringify(str));   // {}
```
上面代码中，对象 `obj` 的 `a` 属性是 `undefined`，而 `b` 属性是一个函数，结果都被 `JSON.stringify()` 过滤。

如果数组的成员是 `undefined`、函数、XML 对象，则这些值会被转成 `null`。
```
var arr = [undefined, function () {alert(1);}];
console.log(JSON.stringify(str));   // "[null,null]"
```
上面代码中，数组 `arr` 的成员是 `undefined`和函数，它们都被转成了 `null`。

正则对象会被转成空对象。
```
JSON.stringify(/foo/)   // "{}"
```
### 第二个参数
`JSON.stringify()`方法还可以接收一个数组，作为第二个参数，指定需要转成字符串的属性。
```
var obj = {
        username: "tom",
        age: 23,
        sex: true
    }
var arr = ["username", "sex"];
console.log(JSON.stringify(obj, arr));   // '{"username":"tom","sex":true}'
```
上面代码中，`JSON.stringify()`方法的第二个参数指定，只转 `username` 和 `sex` 俩个属性。  
这个类似白名单的数组，只对对象的属性有效，对数组无效。
```
console.log(JSON.stringify(["tom", "jack"], 0));   // '["tom","jack"]'
```
第二个参数还可以是一个函数，用来更改 `JSON.stringify()` 的返回值。
```
var obj = {
    a: 1,
    b: 2
}
function fn(key, value){
    if(typeof value === "number"){
        value *= 2;
    }
    return value;
}
console.log(JSON.stringify(obj, fn));   // '{"a":2,"b":4}'
``` 
上面代码中的 `fn` 函数，接收俩个参数，分别是被转换的对象的键名和键值。如果键值是数值，就将它乘以2，否则就原样返回。  
注意，这个处理函数是递归处理所有的键。
```
var o = {
    a: {
        b: 1
    }
}
function fn(key, value) {
    console.log("["+ key + "]"+"---"+ value);
    return value;
}
console.log(JSON.stringify(o, fn));
// []---[object Object]
// [a]---[object Object]
// [b]---1
// '{"a":{"b":1}}'
```
上面代码中，对象 `o` 一共会被 `fn` 函数处理三次: 第一次键为空，值是整个 `o` 对象；第二次键是 `a`，值是 `{b:1}`；第三次键是 `b`，值是 `1`。  
递归处理中，每一次处理的对象，都是前一次返回的值。
```
var o = {a: 1}
function fn(key, value){
    if(typeof value === "object"){
        return {b: 2}
    }
    return value * 2;
}
console.log(JSON.stringify(o, fn));   // '{"b":4}'
```
上面代码中，`fn` 函数修改了对象 `o`，接着，`JSON.stringify()` 方法就递归处理修改后的对象 `o`；  
如果处理函数返回 `undefined`或者没有返回值，则该属性会被忽略。
```
var o = {
    username: "tom",
    age: 24,
    likes: "sing"
}
function fn(key, value){
    if(typeof value === "number"){
        return undefined;
    }
    return value;
}
console.log(JSON.stringify(o, fn));   // '{"username":"tom","likes":"sing"}'
```
上面代码中，`age` 属性经过处理后，返回了 `undefined`，于是该属性被忽略了。
### 第三个参数
`JSON.stringify()` 还可以接收第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数值，表示每个属性前面添加的空格(最多不超过10个)；如果是字符串(不超过10个字符)，则该字符串会添加在每行前面。
```
var o = {username: "tom", age: 25};
console.log(JSON.stringify(o));   // '{"username":"tom","age":25}'
console.log(JSON.stringify(o, null, 2));
// '{
//    "username": "tom",
//    "age": 25
//  }'

console.log(JSON.stringify(o, null, "--"));
// '{
//  --"username": "tom",
//  --"age": 25
//  }'
```
## JSON.parse()
`JSON.parse()` 方法用于将 JSON 字符串转换为对应的值。
```
console.log(JSON.parse("{}"));   // {}
console.log(JSON.parse("true"));   // true
console.log(JSON.parse('"true"'));   // "true"
console.log(JSON.parse('[1, 5, "false"]'));   // [1, 5, "false"]
console.log(JSON.parse('null'));   // "null"
console.log(JSON.parse('{"name": "张三"}'));   // {name: "张三"}
```
如果传入的字符串不是有效的 JSON 格式， `JSON.parse()` 方法将会报错。
```
console.log(JSON.parse('abc'));   // 报错
```
上面代码中，单引号字符串不符合 JSON 格式，所以报错。
为了处理解析错误，可以将 `JSON.parse()` 方法放在 `try...catch` 代码块中。
```
try{
	JSON.parse('abc')
}catch(e){
	console.log(e);
}
```
`JSON.parse()` 可以接收一个处理函数，作为第二个参数，用法和 `JSON.stringify()`方法类似。
```
var str = '{"a": 1, "b": 2}';
function fn (key, value) {
    if (key === "a") {
        return value + 10;
    }
    return value;
}
console.log(JSON.parse(str, fn));   // {a: 11, b: 2}
```
上面代码中，`JSON.parse()` 的第二个参数是一个函数，如果键是 `a`，该函数会将值加上10。
