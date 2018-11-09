# 对象
## 概述
### 生成方法
对象 (object) 是 JavaScript 语言的核心概念，也是最重要的数据类型。

什么是对象？简单说，对象就是一组 "键值对" ( key - value ) 的集合，是一种无序的数据集合。
```
var obj = {
	foo: 'hello',
	bar: 'world'
};
```
上面代码中，大括号就定义了一个对象，它被赋值给变量 `obj`，所以变量 `obj` 就指向一个对象。该对象内部包含俩个键值对 (又称为俩个"成员")，第一个键值对是 `foo: 'hello'`，其中 `foo` 是键名 ( 成员的名称 )，字符串 `hello` 是键值 ( 成员的值 )。键名与键值之间用冒号分隔。俩个键值对之间用逗号分隔。
### 键名
对象的键名都是字符串，所以加不加引号都可以。上面的代码也可以写成下面这样。
```
var obj = {
	'foo': 'hello',
	'bar': 'world'
}
```
如果键名是数值，会被自动转为字符串。
```
var obj = {
    1: 'a'
}
console.log(obj['1']);   // a
```
上面代码中，对象 `obj` 的所有键名虽然看上去像数值，实际上都被自动转成了字符串。
如果键名不符合标识名的条件 ( 比如第一个字符为数字，或者含有空格或者运算符 )，且也不是数字，则必须加上引号，否则会报错。
```
# 报错
var obj = {
	1p: 'hello'
}

# 不报错
var obj = {
	'1 p': 'hello',
	'h w': 'hello',
	'p + q': 'hello'
}
```
上面对象的三个键名，都不符合标识名的条件，所以必须加上引号。

对象的每一个键名又称为 "属性"，它的 "键值" 可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为 "方法"，它可以像函数那样调用。
```
var obj = {
	p: function(x){
		return x * 2;
	}
}
console.log(obj.p(2));   // 4
```
上面的代码中，对象 `obj` 的属性 `p`，就指向了一个函数。

如果属性的值还是一个对象，就形成了链式引用。
```
var o1 = {};
var o2 = {bar: 'hello'};
o1.foo = o2;
console.log(o1.foo.bar);   // hello
```
上面代码中，对象 `o1` 的属性 `foo` 指向对象 `o2`，就可以链式引用 `o2` 的属性。

对象的属性之间用逗号分隔，最后一个属性后面可以加逗号，也可以不加。
```
var obj = {
	a: 1,
	p: function () {...},
}
```
上面的代码中，`m` 属性后面的那个逗号，有没有都可以。

属性可以动态创建，不必在对象声明时就指定。
```
var obj = {};
obj.foo = 123;
console.log(obj.foo);   // 123
```
上面代码中，直接对 `obj` 对象的 `foo` 属性赋值，结果就在运行时创建了 `foo`属性。
### 对象的引用
如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量。
```
var o1 = {};
var o2 = o1;

o1.a = 1;
console.log(o2.a);   // 1

o2.b = 2;
console.log(o1.b);   // 2

console.log(o1);   // {a: 1, b: 2}
console.log(o2);   // {a: 1, b: 2}
```
上面代码中，`o1` 和 `o2`指向同一个对象，因此为其中任何一个变量添加属性，另一个变量都可以读写该属性。

此时，如果取消某一个变量对于原对象的引用，不会影响到另一个变量。
```
var o1 = {};
var o2 = o1;

o1.a = 1;
o2.b = 2;

console.log(o1);   // {a: 1, b: 2}
console.log(o2);   // {a: 1, b: 2}

o1 = 1;
console.log(o1);   // 1
console.log(o2);   // {a: 1, b: 2}
```
上面代码中，`o1` 和 `o2` 指向同一个对象，然后  `o1` 的值变为 `1`，这时不会对 `o2` 产生影响，`o2` 还是指向原来的那个对象。

注意，这种引用只局限于对象，如果俩个变量指向同一个原始类型的值。那么，变量这时都是值的拷贝。
```
var a = 1;
var b = a;

console.log(a);   // 1
console.log(b);   // 1

a = 2;

console.log(a);   // 2
console.log(b);   // 1
```
上面的代码中，当 `a` 的值发生变化后，`b` 的值并不变，这就表示 `a` 和 `b` 并不是指向同一个内存地址。
### 表达式还是语句？
对象采用大括号表示，这导致了一个问题: 如果行首是一个大括号，它到底是表达式还是语句？
```
{foo: 123}
```
JavaScript 引擎读到上面这行代码，会发现可能有俩种含义。第一种可能是，这是一个表达式，表示一个包含 `foo` 属性的对象；第二种可能是，这是一个语句，表示一个代码区块，里面有一个标签 `foo`，指向表达式 `123`。

为了避免这种歧义，V8 引擎规定，如果行首是大括号，一律解释为对象。不过，为了避免歧义，最好在大括号前加上圆括号。
```
({foo: 123})
```
这种差异在 `eval` 语句中反映的最明显。
```
console.log(eval('{foo: 123}'));   // 123
console.log(eval('({foo: 123})'));   // {foo: 123}
```
上面代码中，如果没有圆括号，`eval` 将其理解为一个代码块；加上圆括号以后，就理解成一个对象。
### 属性的操作
#### 属性的读取
读取对象的属性，有俩种方法，一种是使用点运算符，还有一种是使用方括号运算符。
```
var obj = {
    p: 'hello world'
}
console.log(obj.p);   // hello world
console.log(obj['p']);   // hello world
```
上面代码分别采用点运算符和方括号运算符，读取属性 `p`。

注意，如果使用方括号运算符，键名必须放在引号里面，否则会被当做变量处理。
```
var foo = 'bar';
var obj = {
    foo: 1,
    bar: 2
}
console.log(obj.foo);   // 1
console.log(obj[foo]);   // 2
console.log(obj['foo']);   // 1
```

方括号运算符内部还可以使用表达式。
```
var obj = {
    ab: 123
}
console.log(obj['a'+ 'b']);   // 123
```
数字键可以不加引号，因为会自动转成字符串。
```
var obj = {
    0.7: 'hello world'
}
console.log(obj[0.7]);   // hello world
```
上面代码中，对象 `obj` 的数字键 `0.7`，加不加引号都可以，因为会被自动转为字符串。

注意，数值键名不能使用点运算符 (因为会被当成小数点)，只能使用方括号运算符。
```
var obj = {
    0.7: 'hello world'
}
console.log(obj.0.7);   // 报错
console.log(obj[0.7]);   // hello world
```
#### 属性的赋值
点运算符和方括号运算符，不仅可以用来读取值，还可以用来赋值。
```
var obj = {};
obj.foo = 'hello';
obj['bar'] = 'world';
console.log(obj);   // {foo: "hello", bar: "world"}
```
上面代码中，分别使用点运算符和方括号运算符，对属性赋值。

JavaScript 允许属性的 "后绑定"，也就是说，你可以在任意时刻新增属性，没必要在定义对象的时候，就定义好属性。
```
var obj = {
	p: 1
};

// 等价于

var obj = {};
obj.p = 1;
```

#### 属性的查看
查看一个对象本身所有的属性，可以使用 `Object.keys` 方法。
```
var obj = {
    key1: 1,
    key2: 2
};
console.log(Object.keys(obj));   // ["key1", "key2"]
```
#### 属性的删除: delete 命令
`delete` 命令用于删除对象的属性，删除成功后返回 `true`。
```
var obj = {
    key1: 1,
    key2: 2
};
console.log(delete obj.key1);   // true
console.log(obj);   // {key2: 2}
```
上面代码中，`delete` 命令删除对象 `obj` 的 `key1` 属性。删除后，`obj` 对象就只有 `key2` 属性了。

注意，删除一个不存在的属性，`delete` 不报错，而且返回 `true`。
```
var obj = {};
console.log(delete obj.a);   // true
```
上面代码中，对象 `obj` 并没有 `a` 属性，但是 `delete` 命令照样返回 `true`。因此，不能依据 `delete` 命令的结果，认定某个属性是存在的。
#### 属性是否存在: in 运算符
`in` 运算符用于检查对象是否包含某个属性 (注意，检查的是键名，不是键值)，如果包含就返回 `true`，否则返回 `false`。它的左边是一个字符串，表示属性名，右边是一个对象。
```
var obj = {
    a: 123
};
console.log('a' in obj);   // true
console.log('b' in obj);   // false
```
#### 属性的遍历: for...in循环
`for...in` 循环用来遍历一个对象的全部属性。
```
var obj = {
    a: 1,
    b: 2,
    c: 3
};
for (var i in obj) {
    console.log(i + '---' + obj[i]);
}
// a---1
// b---2
// c---3
```

