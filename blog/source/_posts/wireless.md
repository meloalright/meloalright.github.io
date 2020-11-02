---
title: 狐厂鹅厂无线端
date: 2016-12-01
---

## OS

### 缓存

```shell
cache狭义指的是CPU和RAM主存之间的Cache(利用比较昂贵的SRAM) 而且在内存和硬盘之间也有Cache（磁盘缓存） 乃至在硬盘与网络之间也有某种意义上的Cache(如浏览器缓存) 当然也有代码级缓存(某些斐波那契算法) (内存包括ROM RAM Cache)
```

### ASCII

```shell
ASCII = 美国信息交换标准代码

标准: 使用7位二进制数来表示所有的 大写/小写字母 数字0-9 标点符号  美式英语特殊控制字符

0 - NUL空字符
48 - 数字0
65 - A
97 - a

其最高位(b7) == 奇偶校验位:
所谓奇偶校验 是指在代码传送过程中用来检验是否出现错误的一种方法 一般分奇校验和偶校验两种。
奇校验规定: 正确的代码一个字节中1的个数必须是奇数，若非奇数，则在最高位b7添1
偶校验规定: 正确的代码一个字节中1的个数必须是偶数，若非偶数，则在最高位b7添1
```

### unicode

```
Unicode=国际码=国际字符和二进制数字的对应关系
Unicode 只是一个符号集 它只规定了符号的二进制代码 却没有规定这个二进制代码应该如何存储
```

### UTF-8

```js
UTF-8 就是在互联网上使用最广的一种 Unicode 的实现方式。
```

```js
UTF-8 最大的一个特点，就是它是一种变长的编码方式。它可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。
```

```js
UTF-8 的编码规则很简单 只有二条:

1.对于单字节的符号，字节的第一位设为0，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。
2.对于n字节的符号（n > 1），第一个字节的前n位都设为1，第n + 1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的 Unicode 码。

//下面，还是以汉字严为例，演示如何实现 UTF-8 编码。

//严的 Unicode 是4E25（100111000100101）
//根据上表，可以发现4E25处在第三行的范围内（0000 0800 - 0000 FFFF）
//因此严的 UTF-8 编码需要三个字节，即格式是1110xxxx 10xxxxxx 10xxxxxx。
//然后，从严的最后一个二进制位开始，依次从后向前填入格式中的x，多出的位补0。
//这样就得到了，严的 UTF-8 编码是11100100 10111000 10100101，转换成十六进制就是E4B8A5。
```

### URL 编码

```
URL编码 == 百分号编码

对以下类型字符进行编码:
1.引起歧义的字符 (如 value 字符串中包含了 = 或者 & 比附宝洁公司的简称为P&G)
2.非法字符 (URL 的编码格式采用的是 ASCII 码 而不是 Unicode 这也就是说你不能在 URL 中包含任何非 ASCII 字符 例如中文)
```

<!-- more -->

## Network

### HTTP

`使用同一个[TCP连接]来发送和接收多个[HTTP请求/应答]`

```
HTTP 1.0 => 如果浏览器支持 keep-alive => 请求头中添加 Connection: Keep-Alive => 服务器收到请求添加 Connection: Keep-Alive => 保持连接

HTTP 1.1 所有的连接默认都是持续连接 除非特殊声明不支持
```

```
HTTP 2.0 不会线头阻塞 同时通过单一的 HTTP/2 连接发起多重的请求-响应消息
HTTP 2.0 server push 技术 服务器可以对客户端的一个请求发送多个响应
+首部压缩
+二进制分帧层
```

### CGI & WSGI

`CGI: 通用网关接口 连接web服务器和应用程序的接口`  
`WSGI: Python的CGI包装`

### 内网 IP 网段

```ActionScript
但是在IPv4地址协议中预留了3个IP地址段，作为私有地址，供组织机构内部使用。
这三个地址段分别位于A、B、C三类地址内：
A类地址：10.0.0.0--10.255.255.255
B类地址：172.16.0.0--172.31.255.255
C类地址：192.168.0.0--192.168.255.255
```

### http 状态码

```shell
    1**(信息类)：表示接收到请求并且继续处理

    2**(响应成功)：表示动作被成功接收、理解和接受

    3**(重定向类)：为了完成指定的动作，必须接受进一步处理
         301——本网页被永久性转移到另一个URL
         302——请求的网页被转移到一个新的地址。
        ...
         304——自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用

    4**(客户端错误类)：请求包含错误语法或不能正确执行

    5**(服务端错误类)：服务器不能正确执行一个正确的请求
```

### 四种 POST 编码方式

```js
服务端通常是根据请求头（headers）中的 Content-Type 字段来获知请求中的消息主体是用何种方式编码
再对主体进行解析。
```

```
1.application/x-www-form-urlencoded

提交的数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL 转码。


2.multipart/form-data

生成了一个 boundary 用于分割不同的字段


3.application/json

默认提交 JSON 字符串


4.text/xml

使用 XML 作为编码方式的远程调用规范
```

### DNS 污染

```shell
DNS污染主要原理:
用户在访问/请求一个网址时，会将域名提交给DNS来查询这个域名的IP,
查询到域名的IP后才会去访问这个IP请求到想要的资源。
GFW的五种封锁方法中,最简单粗暴的就是DNS污染,
DNS污染的主要原理就是干扰查询域名的返回IP。
而hosts文件优先级比DNS高,hosts文件里若有对应IP,会使计算机绕过DNS直接访问IP。
```

`补充:绕过DNS后如果遇到了IP封锁仍然会访问失败。`

### NAT

`NAT=网络地址转换(Network Address Translation)`  
`wifi=内网ip->公网ip->请求主机`

### 以太网协议

```shell
在以太网协议中规定，同一局域网中的一台主机要和另一台主机进行直接通信，必须要知道目标主机的MAC地址。
而在TCP/IP协议中，网络层和传输层只关心目标主机的IP地址。
这就导致在以太网中使用IP协议时，数据链路层的以太网协议接到上层IP协议提供的数据中，只包含目的主机的IP地址。
于是需要一种方法，根据目的主机的IP地址，获得其MAC地址。这就是ARP协议要做的事情。
所谓地址解析（address resolution）就是主机在发送帧前将目标IP地址转换成目标MAC地址的过程。
另外，当发送主机和目的主机不在同一个局域网中时，即便知道对方的MAC地址，两者也不能直接通信，必须经过路由转发才可以。
所以此时，发送主机通过ARP协议获得的将不是目的主机的真实MAC地址，而是一台可以通往局域网外的路由器的MAC地址。
于是此后发送主机发往目的主机的所有帧，都将发往该路由器，通过它向外发送。这种情况称为委托ARP或ARP代理（ARP Proxy）。
```

## Browser

### 浏览器的 5 个常驻线程

```
浏览器的五个常驻线程：

1.浏览器 GUI 渲染线程

2.javascript 引擎线程

3.浏览器定时器触发线程( setTimeout,setInterval )

4.浏览器事件触发线程

5.浏览器 http 异步请求

当js引擎线程（第二个）进行时，会挂起其他一切线程，这个时候3、4、5这三类线程也会产生不同的异步事件，由于 javascript引擎线程为单线程，所以代码都是先压到队列，采用先进先出的方式运行，事件处理函数，timer函数也会压在队列中，不断的从队头取出事件，这就叫：javascript-event-loop。简单点说应该是当在进行第二线程的时候，1，3，4，5都会挂起，比如这时候触发click事件，即使先前JS已经加载完成，click事件会压在队列里，这里也要先完成第二线程才会执行click事件。
```

### OPTIONS 方法

`区别于GET, POST, DELETE, PUT`

```JavaScript
跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站有权限访问哪些资源。


特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求 比如 Content-Type application/json 的请求:
浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。
服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。

ps: 某些请求不会触发 CORS 预检请求 我们称之为 简单请求
ps: 对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个Origin字段。如果Origin指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。浏览器发现，这个回应的头信息没有包含Access-Control-Allow-Origin字段（详见下文），就知道出错了，从而抛出一个错误，被XMLHttpRequest的onerror回调函数捕获。注意，这种错误无法通过状态码识别，因为HTTP回应的状态码有可能是200。
```

`如何减少OPTIONS预检频率? 答案:Access-Control-Max-Age: 86400`

### 输入 URL 按回车后都发生了什么

```js
-DNS解析 (查询 DNS 浏览器缓存，系统缓存，路由器缓存，IPS服务器缓存，根域名服务器缓存，顶级域名服务器缓存，主域名服务器缓存) ->

-TCP连接 (
 // 3次握手
 // 第一次握手 客户端发送 SYN=> 服务端 (客户端: 我来了)
 // 第二次握手 服务端发送 ACK=> 客户端 (服务端: 好嘞)
 // 第三次握手 客户端发送 ACK=> 服务端 (客户端: 好嘞)

 // 4次挥手
 // 第一次挥手 主动关闭方 FIN=> 被动关闭方 (客户端: 该撤了)
 // 第二次挥手 被动关闭方 ACK=> 主动关闭发 (服务端: 好嘞)
 // 第三次挥手 被动关闭方 FIN=> 主动关闭发 (服务端: 你撤那我也得撤啊 都确认了你最后再吱一声 不然我一直等着就智障了)
 // 第四次挥手 主动关闭方 ACK=> 被动关闭方 (客户端: 好勒)
) ->

-( SSL 四次握手 hello -> certificate-hello -> exchangekey -> finished )

-发送HTTP请求 (HTTP请求报文是由三部分组成: 请求行, 请求报头和请求正文) ->

-服务器处理请求返回HTTP报文 (查找资源返回状态码) ->

-浏览器解析渲染页面 (
 // T0 -> [Get HTML] => [Browser Parsing HTML & prefetch]
 // T1.0 -> [Parsing Dom (Will Parse All The Tags Before Script Soon)]
 // T1.1 -> (Maybe CSS Recieved / JS Recieved) => [(Dom Render Blocking) Load CSS & Build CSSOM] => [Combine CSSOM + DOM -> Render Tree] => [Paint]
 // T1.2 -> [(Dom Parser Blocking) Load JS] => [Run JS] => [Parsing Dom] => (DomContentLoaded Event)
 // T1.3 -> [Composite Layers] => (Load Event)
 ) ->
连接结束
```

### 浏览器渲染流程

```js
// T0
浏览器预下载: 现代浏览器很聪明，会进行 prefetch 优化，浏览器在获得 HTML 文档之后会对页面上引用的资源进行提前下载。
```

```js
// T1.0
DOM解析: 解析器把 HTML 转换为 JavaScript 可存取的 Dom 对象 (由浏览器 UI Render 线程负责)
// T1.1
CSS加载: CSS加载会阻塞 DOM 渲染，不会阻塞 DOM 解析。CSS是阻塞渲染的资源。需要将它尽早、尽快地下载到客户端，以便缩短首次渲染的时间。
Render树: CSSOM 树和 DOM 树合并成渲染树，然后用于计算每个可见元素的布局，并输出给绘制流程，将像素渲染到屏幕上。
// T1.2
JavaScript加载: JavaScript可以查询和修改 DOM 与 CSSOM，所以 JavaScript 执行会阻止 DOM 和 CSSOM 构建 （除非将 JavaScript 显式声明为异步执行）。需要尽量将JavaScript放在 body 的末尾，保证首屏页面先渲染。V8 引擎执行 JavaScript 代码。
DOMContentLoaded Event: 浏览器已经完全加载了HTML，DOM树已经构建完毕，但是像是 <img> 和样式表等外部资源可能并没有下载完毕。
// T1.3
Load Event: 浏览器已经加载了所有的资源（图像，样式表等）。

// Paint 问题
// 经过 Chrome Performance 试验结果:
// 正常情况下 CSS Loaded + Parse StyleSheet (Build CSSOM) 随后会 Paint (即 T1.1 中的 Paint)
// 个别情况下 如 JS 资源下载不耗时 原先 T1.1 中的 Paint 可能会延后到 T1.2 DomContentLoaded Event 之后
```

### Reflow 和 Repaint

```shell
回流: reflow, 浏览器得知元素产生了对文档树排版有影响的样式变化，对所有受影响的dom节点进行重新排版工作
重绘: repaint，就是浏览器得知元素产生了不影响排版的情况下后对这个元素进行重新绘制的过程。例如我们改变了元素的颜色，加个下划线等。

优化思路:
reflow一定触发repaint
repaint不一定触发reflow
因为reflow开销远大于repaint
所以尽量少去触及dom节点重新排版的工作
有时可以直接把改变的元素absolute踢出文档流 reflow+repaint即只涉及这一个节点
```

### 浏览器缓存

```js
1.通过 Meta 标签控制:
例如 - <META HTTP-EQUIV="Pragma" CONTENT="no-cache">
只有部分浏览器支持 并为广泛使用

2.通过 HTTP 缓存控制:
-2.1 强缓存:
 Expires 是服务器端在响应请求时用来规定资源的失效时间。
 Cache-Control (HTTP 1.1 出现) 是服务器端在响应请求时用来规定资源是否需要被浏览器缓存以及缓存的有效时间等。

 Cache-Control 的优先级要高于 Expires

-2.2 协商缓存:
 Etag ETag可以保证每一个资源是唯一的，资源变化都会导致ETag变化。
 Last-Modify 该资源的最后修改时间

 服务器会优先验证ETag
```

### 各大浏览器厂商内核

`IE4-9: Trident`  
`IE10+: EdgeHTML`  
`FireFox: Gecko`  
`Opera: Presto`  
`Safari: Webkit`  
`Chrome: Webkit`

### Tap 与 Click

```js
触屏网页的 tap 与 click :
两者都会在点击时触发，但是在手机WEB端，click会有 200~300 ms，所以请用tap代替click作为点击事件
//(触屏穿透问题最好通过300ms隐形浮层方法解决 或者 fastclick.js)

tap 实现思路:
基于三个基础触摸事件: touchstart、touchmove、touchend
// 滑动则不触发 tap + 长按超时则不触发 tap
```

### LocalStorage SessionStorage 两页面通信

```js
LocalStorage 同源则 => 可以
SessionStorage 同源也 => 不可以
```

### 同源定义

```
[协议]+[端口]+[域名] 都相同
```

## JavaScript

### ES6 ES7 ES8

```js
ES6常用新特性:

1.let && const

2.iterable类型
/**
 *
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();

iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
 */

3.for of
// 用于遍历数组 与forEach()不同的是 它可以正确响应 break continue return 语句

4.解构赋值
// let [a, b, c] = [1, 2, 3];

5.箭头函数
// =>

6....操作符
// Math.max(...[14, 3, 77]) 等同于 Math.max(14, 3, 77);

7.类
// ES6 的 class 可以看作只是一个语法糖
// 类的方法内部如果含有 this 它默认指向类的实例
// 如果一定要实现私有方法可以通过 Symbol 变通实现
const bar = Symbol('bar');
const snaf = Symbol('snaf');

export default class myClass{

  // 公有方法
  foo(baz) {
    this[bar](baz);
  }

  // 私有方法
  [bar](baz) {
    return this[snaf] = baz;
  }

  // ...
};


ES7新特性:

1.Array.prototype.includes

2.Exponentiation Operator(求幂运算)


ES8新特性:
1.Object.values + Object.entries
// let obj = { a: 1, b: 2, c: 3 };
// Object.values(obj) [1,2,3];
// Object.entries(obj) ['a', 1], ['b', 2], ['c', 3]

2.String padding
// 字符串填充

3.Object.getOwnPropertyDescriptors
// writable 属性的值是否可以被重写
// enumerable 此属性是否可以被枚举
// configurable 是否可以删除目标属性或是否可以再次修改属性的特

4.函数参数列表和调用中的尾逗号
// Trailing commas

5.异步函数
// Async Functions
```

### throttle & debounce    

```js
// throttle 节流
// 例如用户8s内连续点击300次按钮 可以throttle(fn, 5000)实现节流5s内只触发一次
module.exports = function throttle(fn, time) {
  var that = fn,
    timer,
    firstTime = true;

  return function () {
    let args = arguments,
      me = this;

    if (firstTime) {
      fn.apply(me, args);
      return (firstTime = false);
    }
    if (timer) {
      return false;
    }

    timer = setTimeout(function () {
      clearTimeout(timer);
      timer = null;
      fn.apply(me, args);
    }, time || 500);
  };
};

// debounce 防抖
// 例如搜索框只有在用户停止输入1s后才出现联想词 deboune(fn, 1000)
module.exports = function debounce(fn, delay) {
  var timer = null;

  return function () {
    clearTimeout(timer);
    timer = setTimeout(function () {
      fn();
    }, delay);
  };
};
```

### ES6 的模块化    

```shell
ES6的模块化的基本规则或特点
1：每一个模块只加载一次， 每一个JS只执行一次， 如果下次再去加载同目录下同文件，直接从内存中读取。 一个模块就是一个单例，或者说就是一个对象；
```

### ES6 import

```
import 命令会被 JavaScript 引擎静态分析 先于模块内的其他语句执行

如果 import 命令要取代 Node 的 require 方法 这就形成了一个障碍
因为 require 是运行时加载模块 import 命令无法取代 require 的动态加载功能
```

### symbol

```
Symbol可以在set对象property的时候保证不会发生覆盖
```

### Array.prototype.sort

```js
//错误用法
//会根据首个char字符assic码排序
Array.prototype.sort.call([12,1,5])
>>> (3) [1, 12, 5]

//正确用法
Array.prototype.sort.call([12,1,5],(a,b)=>(a-b))
>>> (3) [1, 5, 12]

使用Array.prototype.sort不应该只返回两种值 true false 相当于 1 0 而没有 -1
```

### JavaScript This

```js
1.纯粹的函数调用: this === 全局对象
console.log(this === window) // true

2.作为对象方法的调用: 这时 this 就指这个上级对象
x = {
  a: 1,
  f: function() { console.log(this.a) }
}
x.f() //1

3.作为构造函数调用: this 指向这个新对象
function o(n) { this.a = n }
let o1 = new o(1) // {a: 1}

4.call/apply调用: 指向显式调用的对象
x = {
  a: 1,
  f: function() { console.log(this.a) }
}
x.f.call({a:2}) //2

5.箭头函数: 箭头函数不会创建自己的 this 它只会从自己的作用域链的上一层继承 this
x = {
  a: 1,
  f: () => { console.log(this === window) }
}
x.f() //true
```

### Event Loop 机制

```JavaScript
（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。
```

### macrostasks 和 microtasks

```shell
除了广义的同步任务和异步任务，我们对任务有更精细的定义：

macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
micro-task(微任务)：Promise，process.nextTick
```

```js
setTimeout(() => {
  console.log(1);
}, 0);
new Promise((resolve, reject) => {
  resolve("ok");
})
  .then(function () {
    console.log(2);
  })
  .then(function () {
    console.log(3);
  })
  .then(function () {
    console.log(4);
  });
console.log(5);

// >> 5 2 3 4 1
// 主线程的所有代码执行结束后。先从微任务queue里拿回掉函数，然后微任务queue空了后再从宏任务的queue拿函数。
// 事件循环总是 一个宏任务 => 所有微任务 => 一个宏任务 => 所有微任务 ...
```

### setImmediate

```JavaScript
console.log(0);
setImmediate(() => {
    console.log(1)
}, 0)
setTimeout(() => {
    console.log(2)
}, 0)
new Promise((resolve, reject) => {
    resolve('ok')
}).then(() => {
    console.log(3);
})
setTimeout(() => {
    console.log(4)
}, 1000)
process.nextTick(() => {
    console.log(5)
})
console.log(6)

// 0 主执行栈先执行
// 6 主执行栈先执行
// 5 process.nextTick会在事件队列中强势插入
// 3 正常macrotasks执行后该microtasks执行
// 2
// 1 同一时刻setImmediate压入事件队列的顺序会比setTimeout优先级低
// 4

```

### 原型 与 prototype

```JavaScript
function DOG(name){
    this.name = name;
}


DOG.prototype.species = '犬科'; // 尽量不要用改变 prototype 引用的写法 例如: DOG.prototype = { species: '犬科' } ...

var A = new DOG('大毛');
var B = new DOG('二毛');

console.log(A.species); // 犬科
console.log(B.species); // 犬科

DOG.prototype.species = '猫科';

console.log(A.species); // 猫科
console.log(B.species); // 猫科

// 原理: 实例会沿着原型链向上寻找species属性





/**

 @ 引用关系图
                                      ----.constructor-->
 A ---.__proto__--->  DOG.prototype (                     ) DOG
                                      <----.prototype----

 **/

console.log(A.__proto__);
// {species: "猫科", constructor: ƒ}
// 其实就是 DOG.prototype

console.log(DOG.prototype, 'is equal ?', A.__proto__ === DOG.prototype);
// {species: "猫科", constructor: ƒ} 'is equal ?' true
// 就是他

console.log(DOG.prototype === DOG.prototype.constructor.prototype);
// true
// 互相引用



/** @ DOG 和 Function 的关系  **/

console.log(DOG.__proto__);
// ƒ () { [native code] }
// 其实就是 Function.prototype

console.log(Function.prototype, 'is equal ?', DOG.__proto__ === Function.prototype);
// ƒ () { [native code] } "is equal ?" true
// 同理

console.log(Function.prototype === Function.prototype.constructor.prototype)
// true
// 同理




/** @ 所以 对象字面量也同理 **/

let C = { name: "柴犬" };
console.log(C.__proto__ === Object.prototype);
// true

console.log(Object.prototype === Object.prototype.constructor.prototype);
// true
```

### 实现简易 Promise

```JavaScript
// Promise 是包含 pending fufilled rejected 三种状态的状态机 开发者可以指定未来发生的事件

function prototypePromise (excuteFunction) {
    this.stack = [];
    excuteFunction(this.onResolve.bind(this), this.onReject.bind(this));
}



prototypePromise.prototype.then = function (onResolve) {
    this.stack.push(onResolve);
    return this;
}

prototypePromise.prototype.catch = function (handleError) {
    this.handleError = handleError;
    return this;
}

prototypePromise.prototype.onResolve = function (value) {
    let closer = value;
    try {
        Array.prototype.forEach.call(this.stack, (next) => {
            closer = next(closer);
        });
    }
    catch (e) {
        this.onReject(e)
    }
}

prototypePromise.prototype.onReject = function (err) {
    this.handleError(err)
}
```

### JavaScript 内存回收

`1.标记清除: 变量离开执行环境就收回`  
`2.引用计数: 简单粗暴 会受循环引用影响//(中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的， 也就是说只要涉及BOM及DOM就会出现循环引用问题)`

### js 的[按值传递]与[按引用传递]

```js
六种基本数据类型;

undefined;
null;
string;
boolean;
number;
symbol(ES6);

一种引用类型;

Object;
```

```js
//本质原理
(原始数据类型) => 存储在栈中;
//占据空间固定
(引用类型) => 存储在堆中;
//(而存储在变量处的值实际上是指针point,指向存储对象内存地址)
//占据空间大小会变
```

### typeof 对照表

```js
typeof undefined; // undefined
typeof null; // object ☆
typeof true; // boolean
typeof 3; // number
typeof "str"; // string
typeof Symbol("k"); // symbol
typeof { key: 0 }; // object
typeof function () {}; // function
typeof [1, 2, 3]; // object  ☆
```

### 判断是否 Array

```js
let arr = [1, 2, 3];

console.log(arr instanceof Array); // true // es6

Object.prototype.toString.call(arr) === "[object Array]"; // true

arr.constructor === Array; // true
```

### 判断是否空对象

```js
let obj = {};

// 直接 for in + hasOwnProperty
function ieo(obj) {
  for (var i in obj) {
    if (obj.hasOwnProperty(i)) {
      return false;
    }
  }
  return true;
}

ieo(obj); // true

JSON.stringify(obj) === "{}"; // true

Object.keys(obj).length === 0; // true // es6
```

### 如何正确遍历对象属性

```JavaScript
Object.prototype.c = 1; // 把属性c放在 Object 原型链上
let k = { a: 2 }; // 通过字面量创建一个 Object 实例

// $ 错误方法: for in
for (var i in k) {
    console.log(i) // a, c // 会把原型链上的属性也打出来
}

// $ 正确方法: 1.Object.keys
Object.keys(k); // ['a']

// $ 正确方法: 2.hasOwnProperty
k.hasOwnProperty('a') // true
k.hasOwnProperty('c') // false
```

### 箭头函数

```js
1.箭头函数不会创建自己的this 只会从自己的作用域链的上一层继承this ☆

2.通过 call() 或 apply() 方法调用一个函数时 第一个参数会被忽略 (bind也同样成立)

3.不绑定Arguments对象

4.不能和new一起使用 和new一起用会抛出错误
// 因为使用箭头函数后this会指定闭合的当前上下文，而当函数做为构造器的时候，this又会指向生成的实例， 这个造成歧义。

5.没有prototype

6.不能使用yield关键字
```

### XSS

```js
XSS: 跨站脚本攻击
恶意web用户将代码植入到提供给其它用户使用的页面中

XSS防范:
永远不要相信用户的输入 后端转义 + 前端转义
```

### CSRF

```js
CSRF: 跨站请求伪造
一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法

CSRF 防范

1.最基础的防范就是在 Get 与 Post 区别的文章中有说过
Post 请求要比 Get 更为安全 但只能说会 Pass 一些低级攻击选手。

2.请求来源限制——验证 HTTP Referer 字段
在 HTTP 请求头中有一个字段叫 Referer 它记录了请求的来源地址
服务器需要做的是验证这个来源地址是否合法 如果是来自一些不受信任的网站 则拒绝响应
(但是 Refer 也能伪造)

3. CSRFToken
使用随机数作为请求参数来校验
```

### 前端路由原理

```js
hash模式: [hash 改变] + [监听 hashChange] + [route map 执行回调]
history模式: [主动 pushState 改变 Path] + [route map 执行回调]
```

### CSS3 旋转

```CSS
    -webkit-animation: ani-load 0.2s linear infinite;
    animation: ani-load 0.3s linear infinite;
    //
    @-webkit-keyframes ani-load{0%{-webkit-transform:rotate(0deg);transform:rotate(0deg)}
    100%{-webkit-transform:rotate(360deg);transform:rotate(360deg)}
```

### CSS3 transform

```scss
transform: rotate(-25deg) translateX(30px); // 这种先rotate的写法 会把整个坐标系先旋转-25°再平移30px 也就是会向斜上方平移
```

### box-sizing

```HTML
    <div class="wrap-a">
        <p>a</p>
    </div>
    <div class="wrap-b">
        <p>b</p>
    </div>
    <!-- a和b的border-box等价-都能实现占据一行自适应宽度留空间-->
    <!-- 然而实现占据一半自适应宽度留空间 c的border-box是最佳方法-->
    <div class="wrap-c">
        <p>c</p>
    </div>
```

```CSS
        .wrap-a {
            padding: 20px;
            background-color: #CCC;
        }
        .wrap-b {
            width: 100%;
            padding: 20px;
            box-sizing: border-box;
            background-color: #777;
        }
        .wrap-c {
            width: 50%;
            padding: 20px;
            box-sizing: border-box;
            background-color: #EEE;
        }
        p {
            background-color: #444;
        }
```

### 圣杯布局

`经典圣杯`

```HTML
    <div class="wrap">
        <div class="center"></div>
        <div class="left"></div>
        <div class="right"></div>
    </div>
```

```CSS
        .wrap {
            padding: 20px;
        }
        .wrap div {
             float: left;
             height: 30px;
             position: relative;
        }
        .wrap .left {
             width: 20px;
             left: -20px;
             margin-left: -100%;
        }
        .wrap .center {
             width: 100%;
        }
        .wrap .right {
             width: 20px;
             margin-right: -20px;
             background-color: blue;
        }
```

`Flex圣杯`

```HTML
    <div class="container">
        <div class="left"></div>
        <div class="main"></div>
        <div class="right"></div>
    </div>
```

```CSS
    .container{
        display: flex;
        height: 100px;
    }
    .container .left, .container .right{
        flex: 0 0 40px;
    }
    .container .main{
        flex: 1;
    }
```

### 一线天

`中间固定-两端自适应(PC打开移动页面)`

```HTML
    <div class="container">
        <div id="left"></div>
        <div id="main"></div>
        <div id="right"></div>
    </div>
```

```CSS
        #left, #right {
            float: left;
            width: 50%;
            margin: 0 0 0 -70px;
        }

        #main {
            width: 140px;
            float: left;
        }
```

`Flex 一线天`

```HTML
    <div class="container">
        <div id="left"></div>
        <div id="main"></div>
        <div id="right"></div>
    </div>
```

```CSS
        .container {
            display: flex;
        }

        #left, #right {
            flex: 1;
        }

        #main {
            flex: 0 0 140px;
        }
```

### 绝对居中

`经典居中`

```CSS
.container {
    display: relative;
}
.container div {
    position: absolute;    /* 相对定位或绝对定位均可 */
    width: 500px;
    height: 300px;
    top: 50%;
    left: 50%;
    margin: -150px 0 0 -250px;      /* 外边距为自身宽高的一半 */
}
```

`transform居中`

```CSS
.container {
    display: relative;
}
.container div {
    position: absolute;     /* 相对定位或绝对定位均可 */
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

`flex居中`

```CSS
.container {
    display: flex;
    align-items: center;        /* 垂直居中 */
    justify-content: center;    /* 水平居中 */

}
.container div {}
```

### flex property

```scss
flex: flex-grow flex-shrink flex-basis|auto|initial|inherit;

/*
flex-grow   一个数字，规定项目将相对于其他灵活的项目进行扩展的量。
flex-shrink 一个数字，规定项目将相对于其他灵活的项目进行收缩的量。
flex-basis  项目的长度。合法值："auto"、"inherit" 或一个后跟 "%"、"px"、"em" 或任何其他长度单位的数字。
auto    与 1 1 auto 相同。
none    与 0 0 auto 相同。
initial 设置该属性为它的默认值，即为 0 1 auto。请参阅 initial。
inherit 从父元素继承该属性。请参阅 inherit。
*/
```

### jsonp

```javascript
为了便于客户端使用数据，逐渐形成了一种非正式传输协议，人们把它称作JSONP。
该协议的一个要点就是允许用户传递一个callback参数给服务端
然后服务端返回数据时会将这个callback参数作为函数名来包裹住JSON数据
这样客户端就可以随意定制自己的函数来自动处理返回数据了。
```

```javascript
误解: jQuery Zepto之类的库把Ajax方法和jsonp方法封装在一起是容易产生误解的。
jsonp: jsonp实际上就不是ajax方法
jsonp利用<script>标签可以跨域的历史漏洞实现跨域请求参数
服务端将callback参数作为一个js函数的函数名而回调数据作为函数参数返回

callback({ msg:'this is json data'})//jsonp方法的回调
```

### 多核处理上 node 如何榨干处理器资源?

```js
=> 多进程

// 为了将多核 CPU 的性能发挥到极致
// 最大程度地榨干服务器资源
// egg 采用多进程模型
// 解决了一个 Node.js 进程只能运行在一个 CPU 上的问题
```

### Express 原理

```
Express 有几个比较重要的概念: 中间件 路由 模版引擎
```

### Express 中间件原理

```js
app.use:
加载用于处理 http 请求的 middleware 中间件

函数数组:
express 内部维护一个函数数组 这个函数数组表示在发出响应之前要执行的所有函数

结论:
使用 app.use(fn) 后 传进来的fn就会被扔到这个数组里
执行完毕后调用 next() 方法执行函数数组里的下一个函数
如果没有调用 next() 的话 调用就会被终止
```

### Express 路由原理

```js
router对象的主要作用:
创建一个普通中间件 或者 路由中间件的引导着(这个引导着Layer对象链接到一个route对象)
然后将其保存到自己的stack数组中

var express = require('express');
var app = express();

// 没有挂载路径的中间件，应用的每个请求都会执行该中间件
app.use(function (req, res, next) {
  console.log('Time:', Date.now());
  next();
});

// 挂载至 /user/:id 的中间件，任何指向 /user/:id 的请求都会执行它
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method);
  next();
});

// 路由和句柄函数(中间件系统)，处理指向 /user/:id 的 GET 请求
app.get('/user/:id', function (req, res, next) {
  res.send('USER');
});


var server = app.listen(3000, function () {
  var host = server.address().address;
  var port = server.address().port;
  console.log('Example app listening at http://%s:%s', host, port);
});
```

### Express 模板引擎原理

```
ejs / jade 将数据和模板整合最终生成 html 文件
```

### JavaScript 谷歌规范

`[1]声明式和表达式规范`

`结构:应符合[变量-业务逻辑-函数声明]的结构规范`

`[函数声明式]与[函数表达式]在js解释器预编译阶段存在区别`

`[函数声明式]函数声明和他的赋值都会被提前`

```javascript
a(); // >> 1

//声明式
function a() {
  return 1;
}
```

`[函数表达式]和变量一样[声明被提前]而[赋值并不被提前]`

```javascript
console.log(e); // >> undefined

e(); //ERROR:e is not a function(…)

//表达式
var e = function () {
  return 0;
};
```

`结构:应符合[变量-业务逻辑-函数声明]的结构规范`

`[2]注释规范`

`注释:用醒目多行注释对函数表达式的[参数类型]和[返回值类型]进行注释`

```javascript
/**
 * 返回字符的字节长度（汉字算2个字节）
 * @param {string}
 * @returns {number}
 */

var getByteLen = function (val) {
  var len = 0;
  for (var i = 0; i < val.length; i++) {
    if (val[i].match(/[^x00-xff]/gi) != null)
      //全角
      len += 2;
    else len += 1;
  }
  return len;
};
```

## Objective-C

### iOS 中文键盘滚动    

`解决iOS中文键盘滚动的遮挡问题`

```objective-c
#import "ViewController.h"
#import <WebKit/WebKit.h>
#import <objc/runtime.h>

@interface ViewController ()
    @property(strong,nonatomic) WKWebView *webView;
@end

@implementation ViewController


- (void)viewDidLoad {
    [super viewDidLoad];

    _webView = [[WKWebView alloc] initWithFrame:CGRectMake(0, 60, 320, 508)];
    _webView.navigationDelegate = self;
    _webView.scrollView.bounces= NO;
    NSURL *nsurl=[NSURL URLWithString:@"https://XXXXXX..."];

    NSURLRequest *nsrequest=[NSURLRequest requestWithURL:nsurl];
    [_webView loadRequest:nsrequest];
    [self.view addSubview:_webView];

    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(boardWillShow:) name:UIKeyboardWillShowNotification object:nil];

}

-(void)boardWillShow:(NSNotification *)sender{
    //获得键盘的尺寸
    CGRect keyBoardRect=[sender.userInfo[UIKeyboardFrameEndUserInfoKey] CGRectValue];
    _webView.scrollView.contentOffset = CGPointMake(0, keyBoardRect.size.height);
}



- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}



@end
```

### iOS 存储区    

`iOS存储区`

```objective-c
Documents/
//使用这个目录来保存用户生成的文件。用户可以通过文件分享功能访问这个目录的内容。因此，这个目录你应该只放一些你希望展示给用户的文件。这个目录的文件会被 iTunes 和 iCloud 备份。


Library/  (后来发现在wkwebview下会有坑)
//使用 Library 子文件夹来存放任何你不想被用户看到的文件。你的app不应该使用这些文件夹来存放用户数据文件。
//除了 Caches 子文件夹，Library 文件夹下的内容会被 iTunes 和 iCloud 备份。

tmp/
//使用这个文件夹来存放在临时文件，系统也可能会在你的 app 不再运行的时候清除这个文件夹。
//这个目录的文件 不会 被 iTunes 和 iCloud 备份。
```

### ViewController 生命周期

`单个 ViewController 的生命周期`

```shell
loadView: 加载 View

viewDidLoad: 加载 View 完成
viewWillAppear：View 将要显示

viewWillLayoutSubviews：View 将要布局子控件
viewDidLayoutSubviews：View 完成布局子控件
(ps: 此期间这两个 Layout 方法可能会多次重复)

viewDidAppear: View 完全显示
viewWillDisappear: View 即将消失
(ps: 此期间前两个 Layout 方法可能会多次重复)

viewDidDisappear: View 完全消失
```

`多个 ViewControllers 跳转`

```shell
loadView: ViewController2

viewDidLoad: ViewController2

viewWillDisappear: ViewController1 将要消失

viewWillAppear: ViewController2 将要出现

viewWillLayoutSubviews: ViewController2

viewDidLayoutSubviews: ViewController2

viewWillLayoutSubviews: ViewController1

viewDidLayoutSubviews: ViewController1

viewDidDisappear: ViewController1 完全消失

viewDidAppear: ViewController2 完全出现
```

## Algorithm

### JavaScript 类似[列表生成式]版本的[快排]

```js
function qsort(arr) {
  if (arr.length <= 1) {
    return arr;
  }
  var pivot = arr[0];
  var left = [];
  var right = [];
  for (var i = 1; i < arr.length; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }
  return qsort(left).concat([pivot], qsort(right));
}

alert(qsort([32, 45, 37, 16, 2, 87])); //弹出“2,16,32,37,45,87”
```

### JavaScript [空间最优]版本的[快排]     

```JavaScript
Array.prototype.copy = function() {
   return this.constructor.apply(this,this)
}

function qsort (list) {
    if (list.length <= 1) {
        return list
    }
    let copyList = list.copy();
    partial(copyList, 0, copyList.length - 1);
    return copyList;
}

function partial (list, left, right) {
    if (left > right) {
        return;
    }
    let i = left;
    let j = right;
    let pivot = list[left];
    while (i != j) {
        while ((list[j] >= pivot) && i < j) {
            j--
        }
        while ((list[i] <= pivot) && i < j) {
            i++
        }
        if (i < j) {
            let tmp = list[i];
            list[i] = list[j];
            list[j] = tmp;
        }
    }
    let meet = i;
    let tmp = list[meet];
    list[meet] = pivot;
    list[left] = tmp;
    partial(list, left, meet - 1);
    partial(list, meet + 1, right)
}

console.log(qsort([32,45,37,16,2,87]))
```

### JavaScript 归并排序

```JavaScript
function mergeSort(arr){
  if(arr.length < 2){
    return arr;
  }
  var middle = parseInt(arr.length/2);
  var left = arr.slice(0,middle);
  var right = arr.slice(middle);

  return merge(mergeSort(left),mergeSort(right));
}

function merge(left,right){
  var result = [];
  var i = 0, j = 0;
  while(i < left.length && j < right.length){
    if(left[i] > right[j]){
      result.push(right[j++]);
    }
    else{
      result.push(left[i++]);
    }
  }
  while(i < left.length){
    result.push(left[i++]);
  }
  while(j < right.length){
    result.push(right[j++]);
  }

  return result;
}
```

### JavaScript 二分查找

```JavaScript
function bin (list, target) {
    if (list.length <= 1) {
        return list[0] === target? 0: false;
    }
    let left = 0;
    let right = list.length - 1;
    while (left < right) {
        let mid = Math.floor((left + right) / 2);
        let center = list[mid];
        if (center === target) {
            return mid;
        }
        else if (center > target) {
            right = mid
        }
        else {
            left = mid + 1
        }
    }
    return list[left] === target? left: false;
}

bin([11,12,13,14,15,16,17,18,19,20], 18) // 7
```

### JavaScript [递归]版本斐波那契

```JavaScript
function fib (x) {
    if (x == 1) {
        return 1;
    }
    if (x == 2) {
        return 2;
    }
    return fib(x - 2) + fib(x - 1)
}
```

### JavaScript [循环+有缓存]版本斐波那契

```JavaScript
function fib (x) {
    let cache = {
        '1': 1,
        '2': 2
    };

    for (let i = 3; i <= x; i++) {
        cache['' + i] = cache['' + (i - 1)] + cache['' + (i - 2)]
    }
    return cache['' + x]
}
```

### JavaScript [柯里化]版本斐波那契

```JavaScript
function kvcf() {
    let cache = {
        '1': 1,
        '2': 2
    };

    let fib = function (x) {
        if (cache['' + x] !== undefined) { return cache['' + x] }
        return cache['' + x] = fib(x - 2) + fib(x - 1);;
    }
    return fib;
}

console.log(kvcf()(40)); // 165580141
```

### 0 - 3,999,999 长度为 4,000,000 的数组中出现过多少个字符 '1' ？

```Math
求解:
  1000000 / 10 = 100000
  100000 * 6   = 600000
  600000 * 4   = 2400000
  2400000 + 1000000 = 3400000

答案:
  3400000
```

## Framework

### Redux reducer 必须是纯函数

```
reducer 必须是纯函数 必须返回一个新state 不能直接修改老state

本质原因: Redux 判断更新是直接浅层比较 按引用传递会被误认为没有更新
```

### vue 对比 Angular、 React  

```
1. vue的设计倾向于一个Library | 而Angular倾向于一个Framework
2. Vue 3.0 放弃 Object.defineProperty 改用 ES6 Proxy (vue 1.0 - 2.0 的双向绑定定是基于ES5中的getter/setter) | 而Angular基于脏值检测 (从树根部深度遍历效率不高) | React使用setState手动通知数据模型变化
3. Angular使用TypeScript来开发
4. vue更拥抱基于CSS、JavaScript的经典的web开发 学习曲线更友好
5. React会在组件改变时大颗粒度地以其为根组件渲染其所有子组件 | 而vue会跟踪组件关系细粒度地渲染子组件变化
6. React使用JSX以及CSS-in-JS | 而vue即使在单组件内也保留了经典经典的HTML模板以及<style>标签编写CSS
```

### vue 生命周期

```
beforeCreate => created => beforeMount => mounted => beforeDestroy => destroyed
                                           v  ^
                               beforeUpdate -> updated
```

### vue 生命周期 - 数据与 Dom 节点驱动关系

```js
//  <div id="app">
//    <h1>{{message}}</h1>
//  </div>


------beforeCreate------
el     : undefined
data   : undefined
message: undefined

        ------created------   // 此时 data已初始化 dom节点还未创建
        el     : undefined
        data   : {"message":"你好 Vue"}
        message: 你好 Vue






                ------beforeMount------   // 此时 dom节点已创建已添加到页面 {{message}}插值运算还未计算
                el     : <h1>{{message}}</h1>
                data   : {"message":"你好 Vue"}
                message: 你好 Vue

                        ------mounted------  // {{message}}插值运算结束
                        el     : <h1>你好 Vue</h1>
                        data   : {"message":"你好 Vue"}
                        message: 你好 Vue






                        >>> vm.message = '你好世界'



                                ------beforeUpdate------  // data已经改变 virtualdom还未驱动dom更新
                                el     : <h1>你好 Vue</h1>
                                data   : {"message":"你好世界"}
                                message: 你好世界

                                        ------updated------  // 已更新dom
                                        el     : <h1>你好世界</h1>
                                        data   : {"message":"你好世界"}
                                        message: 你好世界


```

### vue observer/dep/watcher 以及 nextTick

```js
Observer: 数据的观察者，让数据对象的读写操作都处于自己的监管之下

Watcher: 数据的订阅者，数据的变化会通知到Watcher，然后由Watcher进行相应的操作，例如更新视图

Dep: Observer与Watcher的纽带，当数据变化时，会被Observer观察到，然后由Dep通知到 Watcher 更新而执行 queuewatcher()


// 示意:

$vm.data => Observer // 一个 data 对应[一个] Observer
                  v
                  Observer.walk // 通过 Observer.walk 多次 defineReactive()
                  v
                  for (..) {
                      const dep = new Dep()..  // 实例化[多个] Dep
                      proxy(data) // Vue 3.0 使用 Proxy 代理多个 key 的 set 和 get
                      dep.depend().. addDep.. addSub.. // 添加[多个]实例化的 watcher 到数组 dep.subs
                  }


----> 拦截到 set ---> Observer 调用 dep.notify()
                                      v
                                      遍历subs去更新
                                      V
                                      for (...) {
                                          queueWatcher() -> -> -> nextTick(flushSchedulerQueue)
                                      }



// nextTick 原理

- queuewatcher 更新本身就用的 nextTick(flushSchedulerQueue)
- Vue.nextTick 依然使用 nextTick 即可保证回调进入事件队列队尾
- nextTick 基于 microTimerFunc (promise) 或者 macroTimerFunc (setImmediate, MessageChannel, setTimeout) 来实现
```

### Angular 在什么时刻更新视图?

```js
// 主要在执行异步事件时
用户输入操作，比如点击，提交等
请求服务端数据
定时事件，比如setTimeout，setInterval

// 如何监听全部异步事件的?
Angular接入了ZoneJS 由它监听了Angular所有的异步事件
// ZoneJS是怎么做到的呢？
其实它重写了所有的异步api（所谓的猴子补丁Monkey patch）！ZoneJS会通知Angular可能有数据发生变化，需要检测更新。
```

### vue key

```js
v-for => 使用就地复用原则
// 数据顺序改变时不会移动 dom
// 而是就地改变元素
// 这个策略不适合类似表单输入框的场景

可以用 :key 来追踪元素
// 类似 vue 1.x 的 track-by
```

### vue 异步组件原理

```
Vue 异步组件只在实际需要渲染组件时 才触发调用工厂函数
```

### Cordova / RN 如何通信

```js
// Cordova
// 通过打开gap://协议请求被native端拦截

... return prompt(argsJson, 'gap:'+JSON.stringify([bridgeSecret, service, action, callbackId]));

... window.webkit.messageHandlers.cordova.postMessage(command);

// native 把返回值和 callbackID 返回给 js
// 完成通信
```

```js
// RN

React Native会在一开始生成OC模块表
然后把这个模块表传入JS中
JS参照模块表 就能通过JavaScriptCore执行而间接调用OC的代码(微信小程序方式类似)
```

## Tips

### weinre 远端调试

```shell
安装
npm install -g weinre
```

```shell
启动
weinre --httpPort 8080 --boundHost -all-
```

### safari 无痕模式

```js
safari无痕浏览模式
window.localStorage.setItem is true but call error
```

### 强类型 弱类型 静态类型 动态类型

```
强类型: 偏向于[不容忍][隐式类型转换]
弱类型: 偏向于[容忍][隐式类型转换]
静态类型: ==[静态类型检查] [编译时]拒绝非法行为
动态类型: ==[动态类型检查] [运行时]拒绝非法行为
```

### 设计模式

```js
1.单体/单例模式: 一个类只能保证有一个实例 例如对象字面量的方式创建一个单例

//先判断实例是否存在，存在则返回，不存在则创建，这样可以保证一个类只有一个实例对象
var test_simple = test_simple || {
   name: 'alice'
}
```

```js
2.1.观察者模式
观察者模式: 要求希望接收到主题通知的观察者必须订阅内容改变的事件

2.2.发布订阅模式
发布订阅模式: 使用了一个主题/事件通道 这个通道介于订阅者和发布者之间 其目的是避免订阅者和发布者产生依赖
```

```js
3.工厂模式
工厂模式: 提供创建对象的接口 封装一些公用的方法 如果实现具体的业务逻辑 可以放在子类重写父类的方法
优点: 弱化对象间的耦合 防止代码重复
缺点: 简单业务可以用 复杂的业务会导致代码维护性差 不易阅读

FruitCake.prototype.makeCake = function(type){
    var cake;
    switch (type){
        case 'apple':
            cake = new AppleCake();break;
        case 'pear':
            cake = new Pear();break;
        default:
            cake = new Orange();break;
    }
    return cake;
}
```

### ES Include Has In

```js
"2" in { "2": 2 }; // true
[1, 2].includes(2); // true
new Set([1, 2]).has(2); // true
```

### (hybrid)app 项目-上线流程

```
开发=>开发打包=>开发自测=>测试环境打包=>测试(测试同学直接去下载测试包)=>正式发版本=>更新到渠道
```

### nvm

```
nvm管理node版本
nvm use -v 0.10.32
```

### package.json 版本固化

`1.2.2 => 大版本号.次要版本号.小版本号`

```js
波浪号（tilde）+指定版本：安装时不改变大版本号和次要版本号。
插入号（caret）+指定版本：安装时不改变大版本号。
latest：安装最新版本。
```

### console.log 的 lazy

```js
控制台打印的[折叠的][按引用传]的对象,把折叠打开后才拿到折叠起来的值。
```

### hybrid-本地 xhr

```
可以在服务端配跨域access-allow-origin为'null'
```

### CORS 如何带 Cookie

```
xhr.withCredentials
 +
Access-Control-Allow-Credentials: true
```

### hybird-base64

```
base64图片为纯本地图片-不需要发网路请求
```

### HTML5 废弃的标签

```js
1.可以使用css代替的标签
删除 basefont,big,center,font,s,strike,tt,u 这些纯表现的元素 html5中提倡把画面展示性功能放在css中统一编辑

2.html5不再使用frame
不再用 frame,noframes和frameset，这些标签对可用性产生负面影响。HTML5中不支持frame框架，只支持iframe框架，或者用服务器方创建的由多个页面组成的符合页面的形式，删除以上这三个标签。

3.只有个别浏览器支持的标签
bgsound 背景音乐，blink 文字闪烁，marquee 文字滚动, applet 用于插入JAVA渲染的代码

4.其他不常用的标签
ul替代dir
pre替代listing
code替代xmp
ruby替代rb
abbr替代acronym
废除isindex使用form与input相结合的方式替代。
废除listing使用pre替代
废除nextid使用guids
废除plaintex使用“text/plian”（无格式正文）MIME类型替代。
```

### CSS3 硬件加速

```js
transform => 不会触发 repaint
// 一点非常类似3D绘图功能
// 最终这些使用 transform 的图层都会由独立的合成器进程进行处理
// opacity filter 也支持硬件加速
```

### CSS 实现 Retina 1px

```css
1.border-image

2.linear-gradient

.border {
    background:
    linear-gradient(180deg, black, black 50%, transparent 50%) top    left  / 100% 1px no-repeat,
    linear-gradient(90deg,  black, black 50%, transparent 50%) top    right / 1px 100% no-repeat,
    linear-gradient(0,      black, black 50%, transparent 50%) bottom right / 100% 1px no-repeat,
    linear-gradient(-90deg, black, black 50%, transparent 50%) bottom left  / 1px 100% no-repeat;
}


3.box-shadow

.hairlines li {
    border: none;
    box-shadow: 0 1px 1px -1px rgba(0, 0, 0, 0.5);
}


4.淘宝使用 initial-scale 0.5/0.333

<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">
<meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.33333, user-scalable=no">


5.伪元素 1px scale(0.5)
```

### node 如何读取一个 2G 的文件

```
一次读取一行 边读边处理边写
```

### Git 冲突

```shell
<<<<<<< HEAD
msg:    db      "HELLO ASM", 10
=======
msg:    db      "Hello man", 10
>>>>>>> origin/as

git会对[不同行修改]自动merge`
git对[同行修改]会冲突`
留下=======作为冲突代码块的区分`
解决冲突:
手动修改conflict的代码`->`git add`->`git commit`->`git push`
```

### Git 切换到远程分支操作

```
git checkout -b apple origin/apple
```

### Git 工作区 暂存区

```js
工作区 (Working Directory):
就是你在电脑里能看到的目录

暂存区 (stage):
工作区有一个隐藏目录 .git 这个不算工作区 而是Git的版本库。
Git的版本库里存了很多东西 其中最重要的就是称为stage（或者叫index）的暂存区

添加+提交 (add+commit)
git add: 把文件添加进去 实际上就是把文件修改添加到暂存区
git commit: 提交更改 实际上就是把暂存区的所有内容提交到当前分支。
一旦提交后 如果你又没有对工作区做任何修改 那么工作区就是"干净"的

(HEAD就是当前活跃分支的游标)
```

### Webpack Cool Plugin

```
1.Tree Shaking: 剔除无用 JS 死代码
2.Webpack Bundle Analyzer: 打包分析
```

### node 调试

```javascript
移动端调试: 直接localhost换成真实ip访问;
```
