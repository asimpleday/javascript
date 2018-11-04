# AJAX
## 简介
浏览器与服务器之间，采用 HTTP 协议通信。用户在浏览器地址栏键入一个网址，或者通过网页表单向服务器提交内容，这时，浏览器就会像服务器发出 HTTP 请求。


具体来说，AJAX 包括以下几个步骤：
1. 创建 XMLHttpRequest 对象
2. 发出 HTTP 请求
3. 接收服务器传回的数据
4. 更新网页数据

概括起来，就是一句话，AJAX 通过原生的 `XMLHttpRequest` 对象发出 HTTP 请求，得到服务器返回的数据后，再进行处理，现在，服务器返回的都是 JSON 格式的数据，XML 格式已经过时了，但是 AJAX 这个名字已经成了一个通用名词，字面含义已经消失了。

`XMLHttpRequest` 对象是 AJAX 的主要接口，用于浏览器与服务器之间的通信，尽管名字里有 XML 和 Http，它实际上可以使用多种协议(比如file和ftp)，发送任何格式的数据(包括字符串和二进制)。

`XMLHttpRequest` 本身是一个构造函数，可以使用 `new`命令生成实例。它没有任何参数。
```
var xhr = new XMLHttpRequest();
```
一旦新建实例，就可以使用  `open` 方法发出 HTTP 请求 。
```
xhr.open("GET", "http://www.example.com/page.php", true);
```
上面代码向指定的服务器网址，发出 GET 请求。
然后指定回调函数，监听通信状态(`readyState`属性)的变化。
```
xhr.onreadystatechange = function (){
	...
}
```
上面代码中，一旦  `XMLHttpRequest` 实例的状态发生变化，就会调用监听指定函数。
一旦拿到服务器返回的数据，AJAX不会刷新整个网页，而是只更新网页中的相关部分，从而不打断用户正在做的事情。

注意，AJAX只能向同源网址(协议、域名、端口都相同)发出 HTTP 请求，如果发出跨域请求，就会报错。

下面是 `XMLHttpRequest`对象简单用法的完整例子。

```
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if(xhr.readyState === 4 && xhr.status === 200){
        console.log(xhr.responseText);
    }
}
xhr.open("GET", "http://www.example.com/page.php", true);
xhr.send();
```

## XMLHttpRequest的实例属性
### 1、XMLHttpRequest.onreadystatechange
`XMLHttpRequest.onreadystatechange` 属性指向一个监听函数，`readystatechange` 事件发生时(实例的readyState属性变化)，就会执行这个函数。

### 2、XMLHttpRequest.readyState
`XMLHttpRequest.readyState`返回一个整数，表示实例对象的当前状态。该属性只读。它可能返回以下值。
+ 0，表示 XMLHttpRequest 实例已经生成，但是实例的 `open()` 方法还没有被调用。
+ 1，表示 `open()`方法已经调用，但是实例的 `send()` 方法还没有被调用，仍然可以使用实例的 `setRequestHeader()`方法，设定 HTTP 请求的头信息。
+ 2，表示实例的`send()`方法已经被调用并且服务器返回的头信息和状态码已经收到。
+ 3，表示正在接收服务器传来的数据体(body部分)。这时，如果实例的 `responseType` 属性等于 `text` 或者空字符串， `respnseText` 属性就会包含已经收到的部分信息。
+ 4，表示服务器返回的数据已经完全接收，或者本次接收已经失败。

通信过程中，每当实例对象发生状态变化，它的 `readyState` 属性的值就会改变，这个值每一次变化，都会触发 `onreadystatechange`事件。
### 3、XMLHttpRequest.response
`XMLHttpRequest.response` 属性表示服务器返回的数据体(即 HTTP 回应的body部分)。它可能是任何数据类型，比如字符串、对象、二进制对象等等，具体的类型由 XMLHttpRequest.responseType 属性决定，该属性只读。

如果本次请求没有成功或者数据不完整，该属性等于 `null`。但是，如果 `responseType` 属性等于 `text` 或者空字符串，在请求没有结束之前(readyState等于3的阶段)，`response` 属性包含服务器已经返回的部分数据。
### 4、XMLHttpRequest.responseType
`XMLHttpRequest.responseType` 属性是一个字符串，表示服务器返回数据的类型。这个属性是可写的，可以在调用 `open()`方法之后，调用`send()`方法之前，设置这个属性的值，告诉服务器返回指定类型的数据。如果 `responseType`属性设为空字符串，就等同于默认值 `text`。

`XMLHttpRequest.responseType` 属性可以等于以下值。
+ ""，空字符串，等同于 `text`，表示服务器返回文本数据。
+ "text"，字符串。
+ "json"， JSON 对象。
+ "document"， Document 对象，表示服务器返回一个文档对象。
+ "blob"， Blob 对象，表示服务器返回二进制对象。
+ "arraybuffer"， ArrayBuffer 对象，表示服务器返回二进制数组。

上面几种类型中， `text`类型适合大多数情况，而且直接处理文本，也比较方便。`document`类型适合返回 HTML / XML 文档的情况，这意味着，对于那些打开 CORS 的网站，可以直接用 AJAX 抓取网页，然后不用解析 HTML 字符串，直接对抓取回来的数据进行 DOM 操作。如果将这个属性设为 `json`，浏览器会自动对返回数据调用 `json.parse()`方法，也就是说，从 `XMLHttpRequest.response`属性(注意：不是是XMLHttpRequest.responseText属性)，得到的不是文本，而是一个 JSON 对象。
### 5、XMLHttpRequest.status
`XMLHttpRequest.status` 属性返回一个整数，表示服务器回应的HTTP状态码，一般来说，通信成功的话，这个状态码是 200，如果服务器没有返回状态码，那么这个属性默认是200，请求发出之前，该属性为0，该属性只读。
+ 200， ok，访问正常
+ 301，Moved Permanently，永久移动
+ 302，Move temporarily，暂时移动
+ 304，Not Modified，未修改
+ 307，Temporary Redirect，暂时从定向
+ 401，Unauthorized，未授权
+ 403，Forbidden，禁止访问
+ 404，Not Found，未发现指定网址
+ 500，Internal Server Error，服务器发生错误
基本上，只有2XX和304的状态码，表示服务器返回的是正常状态。

## XMLHttpRequest 的实例方法
### XMLHttpRequest.open()
`XMLHttpRequest.open()` 用于指定 HTTP 请求的参数，或者说初始化 XMLHttpRequest 实例对象，它一共可以接收5个参数，
```
void open(
   string method,
   string url,
   optional boolean async,
   optional string user,
   optional string password
);
```
+ method : 表示 HTTP 动词方法，比如 GET 、POST、PUT、DELETE、HEAD等。
+ url : 表示请求发送目标 url。
+ async : 布尔值，表示请求是否为异步，默认为 `true`，如果设为 `false`，则 `send()`方法只有等到收到服务器返回了的结果，才会进行下一步操作，该参数可选，由于同步 AJAX 请求会造成浏览器失去响应，许多浏览器已经禁止在主线程使用，只允许 Worker 里面使用，所以，这个参数不应该轻易设为 `false`。
+ user : 表示用于认证的用户名，默认为空字符串，该参数可选。
+ password : 表示用于认证的密码，默认为空字符串，该参数可选。
### XMLHttpRequest.send()
`XMLHttpRequest.send()`用于实际发出 HTTP 请求，它的参数是可选的，如果不带参数，就表示一个 HTTP 请求只有一个 url，没有数据体，典型例子就是 GET 请求，如果带有参数，就表示除了头信息，还带有包含具体数据的信息体，典型例子就是 POST 请求。

下面是 GET 请求的例子。
```
var xhr = new XMLHttpRequest();
xhr.open("GET", "http://www.example.com/?id=" + encodeURIComponent(id));
xhr.send(null);
```
上面代码中，GET 请求的参数，作为查询字符串附加在 url 后面。

下面是发送 POST 请求的例子。
```
var xhr = new XMLHttpRequest();
var data = 'email='
  + encodeURIComponent(email)
  + '&password='
  + encodeURIComponent(password);

xhr.open('POST', 'http://www.example.com', true);
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send(data);
```
注意，所有 XMLHttpRequest 的监听事件，都必须在 `send()` 方法之前设定。
### XMLHttpRequest.setRequestHeader()
`XMLHttpRequest.setRequestHeader()` 方法用于设置浏览器发送的 HTTP 请求的头信息，该方法必须在 `open()`之后、`send()`之前调用。
```
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
```
