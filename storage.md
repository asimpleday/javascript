# Storage 接口
## 概述
Storage 用于脚本在浏览器保存数据。俩个对象部署了这个接口: `window.sessionStorage` 和 `window.localStorage`。
`sessionStorage` 保存的数据用于浏览器的一次会话 ( session )，当会话结束 ( 通常是关闭窗口 )，数据被清空。

`localStorage` 保存的数据长期存在，下一次访问该网站的时候，网页可以直接读取以前保存的数据。除了保存期限的长短不同，这俩个对象的其他方便都一致。

保存的数据都以 "键值对" 的形式存在。也就是说，每一项数据都有一个键名和一个对应的值。所有的数据都是以文本的格式保存。

这个接口很像 Cookie 的强化版，能够使用大得多的存储空间。目前，每个域名的存储上限视浏览器而定，Chrome 是 2.5MB，firefox 和 opera是 5MB，Ie 是 10MB，其中，firefox 的存储空间由一级域名决定，而其他浏览器没有这个限制，也就是说，firefox 中，`a.baidu.com` 和 `b.baiud.com` 共享 5MB 的存储空间。另外，与 Cookie 一样，它们也受同域限制。某个网页存入的数据，只有同域下的网页才能读取，如果跨域操作会报错。
## 属性和方法
Storage 接口只有一个属性。
+ Storage.length: 返回保存的数据项个数。
```
window.localStorage.setItem('foo', 'a');
window.localStorage.setItem('bar', 'b');
window.localStorage.setItem('baz', 'c');
console.log(window.localStorage.length);   // 3
```
该接口提供5个方法。

#### Storage.setItem()
`Storage.setItem()` 用于存入数据，它接收俩个参数，第一个是键名，第二个是保存的数据。如果键名已经存在，该方法会更新的已有的键值。该方法没有返回值。
```
window.sessionStorage.setItem('key', 'value');
window.localStorage.setItem('key', 'value');
```
注意，`Storage.setIem()` 的俩个参数都是字符串，如果不是字符串，会自动转为字符串，再存入浏览器。
```
window.sessionStorage.setItem(3, {foo: 1});
console.log(window.sessionStorage.getItem('3'));   // "[object Object]"
console.log(typeof window.sessionStorage.getItem('3'));   // string
```
上面代码中，`setItem` 方法的俩个参数都不是字符串，但是存入的值都是字符串。

如果存储空间已满，该方法会报错。

写入不一定要用这个方法，直接赋值也是可以的。
```
# 下面三种方法等价
window.localStorage.foo = '123';
window.localStorage['foo'] = '123';
window.localStorage.setItem('foo', '123');
```
#### Storage.getItem()
`Storage.getItem()` 方法用于读取数据。它只有一个参数，就是键名。如果键名不存在，该方法返回 `null`。
```
window.sessionStorage.getItem('key');
window.localStorage.getItem('key');
```
键名应该是一个字符串，否则会被自动转为字符串。
#### Storage.removeItem()
`Storage.removeItem()` 方法用于清除某个键名对应的键值。它接收键名作为参数，如果键名不存在，该方法不会做任何事情。
```
sessionStorage.removeItem('key');
localStorage.removeItem('key');
```
#### Storage.clear()
`Storage.clear()`方法用于清除所有保存的数据。该方法的返回值是 `undefined`。
```
sessionStorage.clear();
localStorage.clear();
```
#### Storage.key()
`Storage.key()` 接收一个整数作为参数 ( 从0开始 )，返回该位置对应的键值。
```
localStorage.setItem('foo', '123');
console.log(localStorage.key(0));   // foo
```
结合使用 `Storage.length` 属性和 `Storage.key()` 方法，可以遍历所有的键。
```
localStorage.setItem('foo', 'a');
localStorage.setItem('bar', 'b');
localStorage.setItem('baz', 'c');

for (var i = 0; i < localStorage.length; i++) {
    console.log(localStorage.key(i));
}
// 'foo'
// 'bar'
// 'baz'
```
## Storage 事件
Storage 接口存储的数据发生变化时，会触发 Storage 事件，可以指定这个事件的监听函数。
```
window.addEventListener('Storage', onStorageChange);
```
监听函数接受一个 `Event` 实例对象作为参数。这个实例对象继承了 Storage.Event 接口，有几个特有的属性，都是只读属性。
+ `StorageEvent.key`: 字符串，表示发生变动的键名。如果 storage 事件是由 `clear()`方法引起，该属性返回 `null`。
+ `StorageEvent.newValue`: 字符串，表示新的键值。如果 storage 事件是由`clear()`方法或删除该键值对引发的，该属性返回`null`。
+ `StorageEvent.oldValue`: 字符串，表示旧的键值。如果该键值对是新增的，该属性返回 `null`。
+ `StorageEvent.url`: 字符串，表示原始触发 storage 事件的那个网页的网址。

下面是 `StorageEvent.key` 属性的例子。
```
function onStorageChange(e) {
  console.log(e.key);
}

window.addEventListener('storage', onStorageChange);

注意，该事件有一个很特别的地方，就是它不在导致数据变化的当前页面触发，而是在同一个域名的其他窗口触发。也就是说，如果浏览器只打开一个窗口，可能观察不到这个事件。比如同时打开多个窗口，当其中的一个窗口导致储存的数据发生改变时，只有在其他窗口才能观察到监听函数的执行。可以通过这种机制，实现多个窗口之间的通信。
```




