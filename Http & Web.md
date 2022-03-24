## 跨域

### （1）CORS

CORS（Corss-Origin Resource Sharing，跨资源共享）基本思想是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应的成功或失败。即给请求附加一个额外的Origin头部，其中包含请求页面的源信息（协议、域名和端口），以便服务器根据这个头部决定是否给予响应。

### （2）Jsonp

```js
// JSONP由两部分组成：回调函数和数据
// 回调函数是接收到响应时应该在页面中调用的函数，其名字一般在请求中指定。
// 数据是传入回调函数中的JSON数据。
var script=document.createElement("script");
script.src="url?callback=handleResponse";
document.body.insertBefore(script,document.body.firstChild);
```

优点：能够直接访问响应文本，可用于浏览器与服务器间的双向通信。

缺点：JSONP从其他域中加载代码执行，其他域可能不安全；难以确定JSONP请求是否失败。

### （2）document.domain

主域相同，子域不同，可以设置document.domain来解决跨域。将页面的document.domain设置为相同的值，页面间可以互相访问对方的JavaScript对象。

注意：不能将值设置为URL中不包含的域；松散的域名不能再设置为紧绷的域名。

```javascript
// 在www.a.com/a.html中
document.domain = 'a.com';
var ifr = document.createElement('iframe');
ifr.src = 'http://www.script.a.com/b.html';
ifr.display = none;
document.body.appendChild(ifr);
ifr.onload = function(){
    var doc = ifr.contentDocument || ifr.contentWindow.document;
    //在这里操作doc，也就是b.html
    ifr.onload = null;
};

// 在www.script.a.com/b.html中
document.domain = 'a.com';
```

### （3）图像Ping

```js
var img=new Image();
img.onload=img.onerror=function(){...}
img.src="url?name=value";
```

请求数据通过查询字符串的形式发送，响应可以是任意内容，通常是像素图或204响应。

图像Ping最常用于跟踪用户点击页面或动态广告曝光次数。

缺点：只能发送GET请求；无法访问服务器的响应文本，只能用于浏览器与服务器间的单向通信。

### （5）Comet

Comet可实现服务器向浏览器推送数据

Comet是实现方式：长轮询和流

- 短轮询即浏览器定时向服务器发送请求，看有没有数据更新。
- 长轮询即浏览器向服务器发送一个请求，然后服务器一直保持连接打开，直到有数据可发送。发送完数据后，浏览器关闭连接，随即又向服务器发起一个新请求。其优点是所有浏览器都支持，使用XHR对象和setTimeout()即可实现。
- 流即浏览器向服务器发送一个请求，而服务器保持连接打开，然后周期性地向浏览器发送数据，页面的整个生命周期内只使用一个HTTP连接。

### （6）WebSocket

```js
// WebSocket可在一个单独的持久连接上提供全双工、双向通信。
// WebSocket使用自定义协议，未加密的连接时ws://；加密的链接是wss://。
var webSocket=new WebSocket("ws://");
webSocket.send(message);
webSocket.onmessage=function(event){
    var data=event.data;
    ... ...
}
```

注意：

- 必须给WebSocket构造函数传入绝对URL；
- WebSocket可以打开任何站点的连接，是否会与某个域中的页面通信，完全取决于服务器；
- WebSocket只能发送纯文本数据，对于复杂的数据结构，在发送之前必须进行序列化JSON.stringify(message))。

优点：在客户端和服务器之间发送非常少的数据，减少字节开销。

## CSRF攻击

CSRF通常从第三方网站发起，被攻击的网站无法防止攻击发生，只能通过增强自己网站针对CSRF的防护能力来提升安全性，防止`csrf`常用方案如下：

- 阻止不明外域的访问：同源检测 + Samesite Cookie
- 提交时要求附加本域才能获取的信息：CSRF Token + 双重Cookie验证

详情参考：https://tech.meituan.com/2018/10/11/fe-security-csrf.html

## https和http

- HTTPS是HTTP协议的安全版本，HTTP协议的数据传输是明文的，是不安全的，HTTPS使用了SSL/TLS协议进行了加密处理，相对更安全
- HTTP 和 HTTPS 使用连接方式不同，默认端口也不一样，HTTP是80，HTTPS是443
- HTTPS 由于需要设计加密以及多次握手，性能方面不如 HTTP
- HTTPS需要SSL，SSL 证书需要钱，功能越强大的证书费用越高

### 混合加密：机密性

对称加密 + 非对称加密：通过两个随机数计算出预主密钥，并通过非对称加密进行通信，然后通过三随机数和预主密钥计算出会话密钥，结合会话密钥使用对称加密进行通信

![image-20220322184112607](/Users/quzhengwei/Library/Application Support/typora-user-images/image-20220322184112607.png)

### 摘要算法：完整性

通过散列函数、哈希函数等压缩算法在原文上附加一个 （SHA-2 的）摘要，网站收到后也计算一下消息的摘要，把这两份“指纹”做个对比，如果一致，就说明消息是完整可信的，没有被修改

### 数字签名：身份认证+不可否定

私钥加密，公钥解密。借助第三方CA验证机构，证明：

- 认证服务器的公开密钥的是真实有效的数字证书认证机构
- 服务器的公开密钥是值得信赖的

其认证主要流程如下：

- 服务器的运营人员向数字证书认证机构提出公开密钥的申请
- 数字证书认证机构在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名（用私钥加密，而且机构的公钥已经提前植入到浏览器中）
- 然后分配这个已签名的公开密钥，并将该公开密钥放入公钥证书后绑定在一起
- 服务器会将这份由数字证书认证机构颁发的数字证书发送给客户端，以进行非对称加密方式通信



