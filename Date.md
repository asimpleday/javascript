# Date 对象
`Date` 对象是 JavaScript 原生的时间库。它以1970年1月1日00:00:00作为时间的零点，可以表示的时间范围是前后各1亿天 (单位是毫秒)。
## 普通函数的用法
`Date` 对象可以作为普通函数直接调用，返回一个代表当前时间的字符串。
```
console.log(Date());   // Fri Nov 09 2018 15:57:35 GMT+0800 
```
注意，即使带有参数，`Date` 作为普通函数使用时，返回的还是当前时间。
```
console.log(Date(2000, 1 ,1));   // Fri Nov 09 2018 16:01:24 GMT+0800
```
上面代码说明，无论有没有参数，直接调用 `Date` 总是返回当前时间。
## 构造函数的使用
`Date` 还可以当作构造函数使用。对它使用 `new` 命令，会返回一个 `Date` 对象的实例。如果不加参数，实例代表的就是当前时间。
```
var today = new Date();
console.log(today);   // Fri Nov 09 2018 16:04:35 GMT+0800
```
`Date` 实例有一个独特的地方。其他对象求值的时候，都是默认调用 `.valueOf` 方法，但是 `Date` 实例求值的时候，默认调用的是 `toString()` 方法。这导致对 `Date` 实例求值，返回的是一个字符串，代表该实例对应的时间。
```
var today = new Date();

today   // Fri Nov 09 2018 16:07:54 GMT+0800

# 等同于
today.toString()
```
上面代码中，`toDay` 是 `Date` 的实例，直接求值等同于调用 `toString` 方法。

作为构造函数时，`Date` 对象可以接收多种格式的参数，返回一个该参数对应的时间实例。
```
# 参数为时间零点开始计算的毫秒数
console.log(new Date(1378218728000));   // Tue Sep 03 2013 22:32:08 GMT+0800

# 参数为日期字符串
console.log(new Date('January 6, 2013'));   // Sun Jan 06 2013 00:00:00 GMT+0800

# 参数为多个整数，分别代表年、月、日、时、分、秒、毫秒
console.log(new Date(2013, 0, 1, 0, 0, 0, 0));   // Tue Jan 01 2013 00:00:00 GMT+0800
```
关于 `Date` 构造函数的参数，有几点说明。

第一点，参数可以是负整数，代表1970年元旦之前的时间。

```
console.log(new Date(-1378218728000));   // Fri Apr 30 1926 17:27:52 GMT+0800
```
第二点，只要是能被 `Date.parse()` 方法解析的字符串，都可以当作参数。
```
new Date('2013-2-15')
new Date('2013/2/15')
new Date('2013-FEB-15')
new Date('FEB, 15, 2013')
new Date('FEB 15,2013')
new Date('Feberuary, 15, 2013')
new Date('Feberuary 15, 2013')
new Date('15 Feb 2013')
new Date('15, Feberuary, 2013')

// Fri Feb 15 2013 00:00:00 GMT+0800
```
上面多种日期字符串的写法，返回的都是同一个时间。

第三，参数为年、月、日等多个整数时，年和月食不能省略的，其他参数都可以省略。也就是说，这时至少需要俩个参数，因为如果只使用 "年" 这一个参数，`Date` 会将其解释为毫秒数。
```
console.log(new Date(2018));   // Thu Jan 01 1970 08:00:02 GMT+0800
```
上面代码中，2018 被解释成毫秒数，而不是年份。
```
new Date(2018, 0)
new Date(2018, 0, 1)
new Date(2018, 0, 1, 0)
new Date(2018, 0, 1, 0, 0, 0, 0)

// Mon Jan 01 2018 00:00:00 GMT+0800
```
上面代码中，不管有几个参数，返回的都是2018年1月1日零点。
最后，各个参数的取值范围如下。
+ 年: 使用四位数年份，比如 `2018`。如果写成俩位数或个位数，则加上 `1900`，即 `10` 代表 1910 年。如果是负数，表示公元前。
+ 月: `0` 表示一月，依次类推，`11` 代表 12 月。
+ 日: `1` 到 `31`。
+ 时: `0` 到 `23`。
+ 分: `0` 到 `59`。
+ 秒: `0` 到 `59`。
+ 毫秒: `0` 到 `999`。

注意，天数从 `1` 开始计算。月、时、分、秒、毫秒全部从 `0` 开始计算。

这些参数如果超出了正常范围，会被自动折算。比如，如果月份设为 `15`，就折算为下一年的 `4` 月。
```
console.log(new Date(2018, 15));   // Mon Apr 01 2019 00:00:00 GMT+0800

console.log(new Date(2018, 0, 0));   // Sun Dec 31 2017 00:00:00 GMT+0800
```
上面第一行代码设置月份月15，就折算成明年的 `4` 月 (注意，`11` 代表 `12` 月)，第二行代码日期设置为0，就折算上个月的最后一天。

参数还可以使用负数，表示扣去的时间。
```
console.log(new Date(2018, -1));   // Fri Dec 01 2017 00:00:00 GMT+0800

console.log(new Date(2018, 0, -1));   // Fri Dec 01 2017 00:00:00 GMT+0800
```
上面代码中，分别对月和日使用了负数，表示从基准日扣去相应的时间。
## 日期的运算
类型自动转换时，`Date` 实例如果转为数值，则等于对应的毫秒数；如果转为字符串，则等于对应的日期字符串。所以，俩个日期实例对象进行减法运算时，返回的是它们间隔的毫秒数，返回的是俩个字符串连接而成的新字符串。
```
var d1 = new Date(2018, 0, 1);
var d2 = new Date(2018, 0, 2);
console.log(d2 - d1);   // 86400000
console.log(d1 + d2);   // Mon Jan 01 2018 00:00:00 GMT+0800 Tue Jan 02 2018 00:00:00 GMT+0800
```
## 静态的方法
### Date.now()
`Date.now()` 方法返回当前时间距离时间零点 ( 1970年1月1日00:00:00 UTC ) 的毫秒数，相当于 Unix 时间戳乘以 1000。
```
console.log(Date.now());   // 1541767216072
```
### Date.parse()
`Date.parse()` 方法用来解析日期字符串，返回该时间距离时间零点的毫秒数。
```
Date.parse('Aug 9, 1995')
Date.parse('January 26, 2011 13:51:50')
Date.parse('Mon, 25 Dec 1995 13:30:00 GMT')
Date.parse('Mon, 25 Dec 1995 13:30:00 +0430')
Date.parse('2011-10-10')
Date.parse('2011-10-10T14:48:00')
```
上面的日期字符串都可以解析。

如果解析失败，返回 `NaN`。
```
console.log(Date.parse('aaa'));   // NaN
```
### Date.UTC()
`Date.UTC()`方法接收年、月、日等变量作为参数，返回该时间距离时间零点的毫秒数。
```
#格式
Date.UTC(year, month[, date[, hrs[, min[, sec[, ms]]]]])

#例子
console.log(Date.UTC(2018, 0, 1, 0, 0, 0));   // 1514764800000
```
该方法的参数用户与 `Date` 构造函数完全一致，比如月份从 `0` 开始计算，日期从 `1` 开始计算，区别在于`Date.UTC`方法的参数，会被解释为 UTC 时间 (世界标准时间)，`Date` 构造函数的参数会被解释为当前时区的时间。

## 实例方法
`Date` 的实例对象，有几十个自己的方法，除去 `valueOf` 和 `toString`，可以分为以下三类。
+ to类: 从 `Date`对象返回一个字符串，表示指定的时间。
+ get类: 获取 `Date` 对象的日期和时间。
+ set类: 设置 `Date` 对象的日期和时间。
### Date.prototype.valueOf()
`valueOf` 方法返回实例对象距离时间零点对应的毫秒数，该方法等同于 `getTime`方法。
```
var d = new Date();
console.log(d.valueOf());   // 1541819124704
console.log(d.getTime());   // 1541819124704
```
预期为数值的场合，`Date` 实例会自动调用该方法，所以可以用下面的方法计算时间的间隔。
```
var d1 = new Date();
// ...
var d2 = new Date();
console.log(d2 - d1);   // 0
```
### to 类方法
#### (1)、Date.prototype.toString()
`toString` 方法返回一个完整的日期字符串。
```
var d = new Date(2018, 0, 1);
console.log(d.toString());   // "Mon Jan 01 2018 00:00:00 GMT+0800"
因为 `toString()` 是默认的调用方法，所以如果直接读取`Date` 实例，就相当于调用这个方法。
```
#### (2) Date.prototype.toUTCString()
`toUTCString` 方法返回对应的 UTC 时间，也就是北京时间晚 8 个小时。
```
var d = new Date(2018, 0, 1);
console.log(d.toUTCString());   // Sun, 31 Dec 2017 16:00:00 GMT
```
#### (3) Date.prototype.toDateString()
`toDateString()` 方法返回日期字符串(不含小时、分、秒)。
```
var d = new Date(2018, 0, 1);
console.log(d.toDateString());   // Mon Jan 01 2018
```
#### (4) Date.prototype.toTimeString()
`toTimeString()` 方法返回时间字符串 (不含年月日)。
```
var d = new Date(2018, 0, 1);
console.log(d.toTimeString());   // 00:00:00 GMT+0800
```
#### (5) 本地时间
以下三种方法，可以将 Date 实例转为 表示本地时间的字符串。
+ `Date.prototype.toLocaleString()`: 完整的本地时间。
+ `Date.prototype.toLocaleDateString()`: 本地时期 (不含小时、分、秒)。
+ `Date.prototype.toLocaleTimeString()`: 本地时间 (不含年、月、日)。

下面是用法实例。
```
var d = new Date(2018, 0, 1);

console.log(d.toLocaleString());   // 2018/1/1 上午12:00:00

console.log(d.toLocaleDateString());   // 2018/1/1

console.log(d.toLocaleTimeString());   // 上午12:00:00
```
这三个方法都有俩个可选的参数。
```
dateObj.toLocaleString([locales, [options]])
dateObj.toLocaleDateString([locales, [options]])
dateObj.toLocaleTimeString([locales, [options]])
```
这俩个参数中，`locales` 是一个指定所用语言的字符串，`options` 是一个配置对象。下面是 `locales` 的例子。
```
var d = new Date(2018, 0, 1);

console.log(d.toLocaleString('es-US'));   // 1/1/2018 12:00:00 a. m.
console.log(d.toLocaleString('zh-CN'));   // 2018/1/1 上午12:00:00

console.log(d.toLocaleDateString('es-US'));   // 1/1/2018
console.log(d.toLocaleDateString('zh-CN'));   // 2018/1/1

console.log(d.toLocaleTimeString('es-US'));   // 12:00:00 a. m.
console.log(d.toLocaleTimeString('zh-CN'));   // 上午12:00:00
```
下面是 `options` 的例子。
```
var d = new Date(2013, 0, 1);

// 时间格式
// 下面的设置是，星期和月份为完整文字，年份和日期为数字
d.toLocaleDateString('en-US', {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
})
// "Tuesday, January 1, 2013"

// 指定时区
d.toLocaleTimeString('en-US', {
  timeZone: 'UTC',
  timeZoneName: 'short'
})
// "4:00:00 PM UTC"

d.toLocaleTimeString('en-US', {
  timeZone: 'Asia/Shanghai',
  timeZoneName: 'long'
})
// "12:00:00 AM China Standard Time"

// 小时周期为12还是24
d.toLocaleTimeString('en-US', {
  hour12: false
})
// "00:00:00"

d.toLocaleTimeString('en-US', {
  hour12: true
})
// "12:00:00 AM"
```

### get 类方法
`Date` 对象提供了一系列 `get` 方法，用来获取实例对象某个方面的值。
+ `getTime()`: 返回实例距离1970年1月1日00：00：00的毫秒数，等同于`
valueOf` 方法。
+ `getDate()`: 返回实例对象对应每个月的几号(从1开始)。
+ `getDay()`: 返回星期几，星期一到星期日，就是 1、2、3、4、5、6、0。
+ `getYear()`: 返回距离1900的年数。
+ `getFullYear()`: 返回四位数的年份。
+ `getMonth()`: 返回月份，1月到12月，分别是0到11。
+ `getHours()`: 返回小时，0到23。
+ `getMilliseconds()`: 返回毫秒数，0到999。
+ `getMinutes()`: 返回分钟，0到59。
+ `getSeconds`: 返回秒，0到59。

```
# 获取现在的时间

var d = new Date();
var year = d.getFullYear();
var month = d.getMonth();
var day = d.getDate();
var hours = d.getHours();
var minutes = d.getMinutes();
var seconds = d.getSeconds();
console.log("现在时间为: " + year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds);

// 现在时间为: 2018-10-10 14:57:29
```
```
# 计算本年度还剩下多少天

function leftDays() {
    var toDay = new Date();
    var endYear = new Date(toDay.getFullYear(), 11, 31, 23, 59, 59, 999);
    var msPerDay = 1000 * 60 * 60 *24;
    return Math.round((endYear.getTime() - toDay.getTime()) / msPerDay);
}
console.log(leftDays());   // 51
```
上面这些 `get` 方法返回的都是当前时区的时间，`Date` 对象还提供了这些方法对应的 UTC 版本，用来返回 UTC 时间。

+ `getUTCDate()`
+ `getUTCFullYear()`
+ `getUTCMonth()`
+ `getUTCDay()`
+ `getUTCHours()`
+ `getUTCMinutes()`
+ `getUTCSeconds()`
+ `getUTCMilliseconds()`

### Set 类方法
`Date` 对象提供了一些列 `set` 方法，用来设置实例对象的各个方面。
+ `setDate(data)`: 设置实例对象对应的每个月的几号，(1-31)，返回改变后毫秒时间戳。
+ `setYear(year)`: 设置距离1900年的年数。
+ `setFullYear(year [, month, date])`: 设置四位数年份。
+ `setHours(hours[, min, sec, ms])`: 设置小时，0-23。
+ `setMilliseconds(ms)`: 设置毫秒，0-999。
+ `setMinutes(min[, sec, ms])`: 设置分钟，0-59。
+ `setMonth(month[, date])`: 设置月份，0-11。
+ `setSeconds(sec[, ms])`: 设置秒数，0-59。
+ `setTime(milliseconds)`: 设置毫秒时间戳。
这些方法，几乎跟 `get`方法一一对应的，但是没有 `satDay` 方法，因为星期几是计算出来的，而不是设置的。另外，需要注意的是，凡是涉及到设置月份，都是从 `0` 开始计算的，即 `0` 是 `1` 月， `11` 是 `12`月。
```
var d = new Date(2018, 0, 1);
console.log(d);   // Mon Jan 01 2018 00:00:00 GMT+0800
console.log(d.getTime());   // 1514736000000
d.setDate(2);
console.log(d.getTime());   // 1514822400000
```
`set` 方法的参数都会自动折算。以 `setDate`为例，如果参数超过当月的最大天数，则会下一个月顺延，如果参数是负数，表示从这个月的最后一天开始减去的天数。
```
var d = new Date(2018, 0, 1);
console.log(d);   // Mon Jan 01 2018 00:00:00 GMT+0800
console.log(d.setDate(32));   // 1517414400000
console.log(d);   // Thu Feb 01 2018 00:00:00 GMT+0800
console.log(d.setDate(-1));   // 1517241600000
console.log(d);   // Tue Jan 30 2018 00:00:00 GMT+0800
```
`set`类方法和 `get`类方法，可以结合使用，得到相对时间。
```
var d = new Date();

// 将日期往后推1000天
d.setDate(d.getDate() + 1000);

// 将时间设为6小时后
d.setHours(d.getHours() + 6);

// 将年份设为去年
d.setFullYear(d.getFullYear() - 1);
```
`set`系列方法除了 `setTime()` 和 `setYear()`之外，都有对应的 UTC 版本，即设置 UTC 时区的时间。
+ `setUTCDate()`
+ `setUTCFullYear()`
+ `setUTCHours()`
+ `setUTCMilliseconds()`
+ `setUTCMinutes()`
+ `setUTCMonth()`
+ `setUTCSeconds()`
```
var d = new Date('January 6, 2013');
d.getUTCHours() // 16
d.setUTCHours(22) // 1357423200000
d // Sun Jan 06 2013 06:00:00 GMT+0800 (CST)
```
上面代码中，本地时区（东八时区）的1月6日0点0分，是 UTC 时区的前一天下午16点。设为 UTC 时区的22点以后，就变为本地时区的上午6点。









### 月份英语及简写
+ 一月: January --- Jan
+ 二月: February --- Feb
+ 三月: March --- Mar
+ 四月: April --- Apr
+ 五月: May --- May
+ 六月: June --- Jun
+ 七月: July --- Jul
+ 八月: August --- Aug
+ 九月: September --- Sep
+ 十月: October --- Oct
+ 十一月: November --- Nov
+ 十二月: December --- Dec




