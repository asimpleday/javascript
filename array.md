# 数组
### 定义
数组 (array) 是按照次序排列的一组值。每个值的位置都有编号 ( 从0开始 )，整个数组用方括号表示。
```
var arr = ['a', 'b', 'c'];
```
上面代码中的 `a`、`b`、`c`就构成了一个数组，俩端的方括号是数组的标志。`a` 是 0 号位置，`b` 是 1 号位置，`c` 是 2 号位置。

除了在定义时赋值，数组也可以先定义后赋值。
```
var arr = [];
arr[0] = 'a';
arr[1] = 'b';
arr[2] = 'c';
console.log(arr);   // [ "a", "b", "c" ]
```
任何类型的数据，都可以放入数组。
```
var arr = [
    {a: 1},
    [1, 2, 3],
    function () {
        return true;
    }
];
console.log(arr[0]);   // {a: 1}
console.log(arr[1]);   // [1, 2, 3]
console.log(arr[2]);   // function () {return true;}
```
上面数组 `arr` 的3个成员依次是对象、数组、函数。

如果数组的元素还是数组，就形成了多维数组。
```
var arr = [[1, 2], [3, 4]];
console.log(arr[0][0]);   // 1
console.log(arr[1][1]);   // 4
```
### 数组的本质
本质上，数组属于一种特殊的对象。`typeof` 运算符会返回数组的类型 `object`。
```
var arr = [];
console.log(typeof arr);   // object
```
上面代码表明，`typeof` 运算符认为数组的类型就是对象。

数组的特殊性体现在，它的键是按次序排列的一组整数。
```
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr));   // [ "0", "1", "2" ]
```
上面代码中，`Object.keys` 方法返回数组的所有键。可以看到数组的键就是整数 0、1、2。

由于数组成员的键是固定的 (默认总是0、1、2...)，因此数组不用为每个元素指定键，而对象的每个成员都必须指定键。JavaScript 语言规定，对象的键一律为字符串，所以，数组的健其实也是字符串。之所以用数值读取，是因为非字符串的键会被转为字符串。
```
var arr = ['a', 'b', 'c'];
console.log(arr['0']);   // a
console.log(arr[0]);   // a
```
上面代码分别用数值和字符串作为键，结果都能读取数组。原因是数值键被自动转为了字符串键。

注意，这点在赋值时也成立。如果一个值总是先转成字符串，再进行赋值。
```
var arr = [];
arr[1.00] = 6;
console.log(arr[1]);
```
上面代码中，由于 `1.00` 转成字符串是 `1`，所以通过数字键 `1` 可以读取值。

对象有俩种读取成员的方法: 点结构 ( `object.key` ) 和方括号结构 ( `object[key]` )。但是，对于数值的键，不能使用点结构。
```
var arr = ['a', 'b', 'c'];
console.log(arr.0);   // after argument list
```
上面代码中，`arr.0` 的写法不合法，因为单独的数值不能作为标识符。所以，数组成员只能用方括号 `arr[0]` 表示 (方括号是运算符，可以接收数值)。
### length 属性
数组的 `length` 属性，返回数组的成员数量。
```
var arr = ['a', 'b', 'c'];
console.log(arr.length);   // 3
```
只要是数组，就一定有 `length` 属性。该属性是一个动态的值，等于键名中的最大整数加上 `1`。
```
var arr = ['a', 'b'];
console.log(arr.length);   // 2

arr[100] = 'he';
console.log(arr.length);   // 101
```
上面代码表示，数组的数字键不需要连续，`length` 属性的值总是比最大的那个整数键大 `1`。另外，这也表明数组是一种动态的数据结构，可以随时增减数组的成员。 

length 属性是可写的。如果人为设置一个小于当前成员个数的值，该数组的成员会自动减少到 `length` 设置的值。
```
var arr = ['a', 'b', 'c', 'd', 'e'];
arr.length = 3;
console.log(arr);   // [ "a", "b", "c" ]
```
上面代码表示，当数组的 `length` 属性设为 `3`，那么数组只有 `3` 个成员了，原数组最后俩项被自动删除了。

清空数组的一个有效方法，就是将 `length` 属性设为 `0`。
```
var arr = ['a', 'b', 'c', 'd', 'e'];
arr.length = 0;
console.log(arr);   // []
```
如果人为设置 `length` 大于当前元素个数，则数组的成员数量会增加到这个值，新增的位置都是空位。
```
var arr = ['a', 'b', 'c'];
arr.length = 5;
console.log(arr);   // [ "a", "b", "c", <2 empty slots> ]
```
### in 运算符
检查某个键名是否存在的运算符 `in`，适用于对象，也适用于数组。
```
var arr = ['a', 'b', 'c'];
console.log(2 in arr);   // true
console.log('2' in arr);   // true
console.log(5 in arr);   // false
```
注意，如果数组的某个位置是空位，`in`运算符返回 `false`。 
```
var arr = [];
arr[100] = 'a';
console.log(1 in arr);   // false
console.log(100 in arr);   // true
```
### 数组的遍历
#### (1)、for...in 循环
`for...in` 循环不仅可以遍历对象，也可以遍历数组，毕竟数组只是一种特殊对象。
```
var arr = ['a', 'b', 'c'];
for (var i in arr) {
    console.log(i + '---' + arr[i]);
}
// 0---a
// 1---b
// 2---c
```
#### (2)、for 循环
```
var arr = ['a', 'b', 'c'];
for (var i = 0; i < arr.length; i ++) {
    console.log(i + '---' + arr[i]);
}
// 0---a
// 1---b
// 2---c
```
#### (3)、while 循环
```
var arr = ['a', 'b', 'c'];
var i = 0;
while(i < arr.length){
    console.log(i + '---' + arr[i]);
    i ++;
}
// 0---a
// 1---b
// 2---c
```
#### (4)、逆向遍历
```
var arr = ['a', 'b', 'c'];
var i = arr.length;
while(i --){
    console.log(i + '---' + arr[i]);
}
// 0---a
// 1---b
// 2---c
```
逆向遍历就是从最后一个元素向第一个元素遍历。
#### (5)、forEach
数组的 `forEach` 方法，也可以用来遍历数组。
```
var arr = ['a', 'b', 'c'];
arr.forEach(function (item, index) {
    console.log(index + '---' + item);
})
// 0---a
// 1---b
// 2---c
```
### 数组的空位
当数组的某个位置是空元素，即俩个逗号之间没有任何值，我们称该数组存在空位。
```
var arr = ['a',,'c'];
console.log(arr.length);   // 3
```
上面代码表明，数组的空位不影响 `length` 属性。

需要注意的是，如果最后一个元素后面有逗号，并不会产生空位。也就是说，有没有这个逗号，结果都是一样的。
```
var arr = ['a', 'b', 'c',];
console.log(arr);   // [ "a", "b", "c" ]
console.log(arr.length);   // 3
```
上面代码中，数组最后一个成员后面有一个逗号，这不影响 `length` 属性的值，与没有这个逗号时效果一样。

数组的空位是可以读取的，返回 `undefined`。
```
var arr = [,,,];
console.log(arr[0]);   // undefined
```

使用 `delete` 命令删除一个数组成员，会形成空位，并且不会影响 `length` 属性。
```
var arr = ['a', 'b', 'c'];
delete arr[1];
console.log(arr);   // [ "a", <1 empty slot>, "c" ]
console.log(arr.length);   // 3
```
上面代码用 `delete` 命令删除了数组的第二个元素，这个位置就形成了空位，但是对 `length` 属性没有影响。也就是说，`length` 属性不过滤空位。所以，使用 `length` 属性进行数组遍历，一定要非常小心。

数组的某个位置是空位，与某个位置是 `undefined`，是不一样的。如果是空位，使用数组的 `forEach`方法、`for...in` 结构、以及 `Object.keys`方法进行遍历，空位都会被跳过。
```
var arr = [,,,];
arr.forEach(function (item, index) {
    console.log(item);   // 不会有任何反应
})

for (var i in arr) {
    console.log(arr[i]);   // 不会有任何反应
}

console.log(Object.keys(arr));   // []
```
如果某个位置是 `undefined`，遍历的时候就不会被跳过。
```
var arr = [undefined, undefined, undefined];
arr.forEach(function (item, index) {
    console.log(index + '---' + item);
})
// 0---undefined
// 1---undefined
// 2---undefined

for (var i in arr) {
    console.log(i);
}
// 0
// 1
// 2

console.log(Object.keys(arr));   // [ "0", "1", "2" ]
```
这就是说，空位就是数组没有这个元素，所以不会被遍历到，而 `undefined` 则表示数组有这个元素，值是 `undefined`，所以遍历不会跳过。
### 类似数组的对象
如果一个对象的所有键名都是正整数或零，并且有 `length` 属性，那么这个对象就很像数组，语法上称为 "类似数组的对象"。
```
var obj = {0: 'a', 1: 'b', 2: 'c', length: 3};
console.log(obj[0]);   // a
console.log(obj.length);   // 3
obj.push('d');   // obj.push is not a function
```
"类似数组的对象"的根本特征，就是具有 `length` 属性。只要有 `length` 属性，就可以认为这个对象类似于数组。但是又一个问题，这种 `length` 属性不是动态值，不会随着成员的变化而变化。
```
var obj = {
	length: 0
}
obj[3] = 'd';
console.log(obj.length);   // 0
```
上面代码为对象 `obj` 添加了一个数字键，但是 `length` 属性没变。这就说明了 `obj` 不是数组。

典型的 "类似数组的对象" 是函数的 `arguments` 对象，以及大多数 `DOM` 元素集，还有字符串。
```
#arguments
function args(a, b) {
    return arguments;
}
var arrayLike = args('a', 'b');
console.log(arrayLike[0]);   // a
console.log(arrayLike.length);   // 2
console.log(arrayLike instanceof Array);   // false

# DOM 元素集
var elts = document.getElementsByTagName("h3");
console.log(elts.length);   // 3
console.log(elts instanceof Array);   // false

# 字符串
var str = 'hello world';
console.log(str.length);   // 11
console.log(str instanceof Array);   // false
```
上面代码包含三个例子，它们都不是数组 ( `instanceof` 运算符返回 `false` )，但是看上去都非常像数组。

数组的 `slice` 方法可以将 "类似数组的对象" 变成真正的数组。
```
var str = 'hello';
var arr = Array.prototype.slice.call(str);
console.log(arr);   // [ "h", "e", "l", "l", "o" ]
console.log(arr.length);   // 5
console.log(arr instanceof Array);   // true
```
除了转为真正的数组，"类似数组的对象"还有一个办法可以使用数组的方法，就是通过 `call()` 把数组的方法放到对象上面。
```
var str = 'hel';
function print(value, index) {
    console.log(index + ' : ' + value);
}
Array.prototype.forEach.call(str,print);
// 0 : h
// 1 : e
// 2 : l
```
上面代码中，`str` 代表一个类似数组的对象，本来是不可以使用数组的 `forEach()`方法的，但是通过 `call()`，可以把 `forEach()` 嫁接到 `str` 上面调用。

下面的例子就是通过这种方法，在 `arguments` 对象上面调用 `forEach` 方法。
```
#forEach 方法
function logArgs() {
    Array.prototype.forEach.call(arguments, function (elem, i) {
        console.log(i + ' : ' + elem);
    })
}
# 等同于 for 循环
function logArgs() {
    for (var i = 0; i < arguments.length; i ++) {
        console.log(i + ' : ' + elem);
    }
}

logArgs('a', 'b', 'c');
// 0 : a
// 1 : b
// 2 : c
```
注意，这种方法比直接使用数组原生的 `forEach` 要慢，所以最好还是先将 "类似数组的对象" 转为真正的数组，然后再直接调用数组的 `forEach` 方法。
```
var str = 'hel';
var arr = Array.prototype.slice.call(str);
arr.forEach(function (elem, i) {
    console.log(i + ' : ' + elem);
})
// 0 : h
// 1 : e
// 2 : l 
```


