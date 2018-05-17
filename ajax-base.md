---
title: ajax基础学习
date: 2018-05-05 21:11:46
tags: [ajax]
categories: 学习
---
- 创建XMLHttpRequest对象：
 <!-- more -->
```js
/**
 * 检测原生XHR是否存在
 * 是：返回XHR实例
 * 否：检测ActiveX对象
 * 如果两种对象都不存在，抛出错误
 */
function createXHR(){
  if(typeof XMLHttpRequest != 'undefined'){ // 兼容ie7+
    return new XMLHttpRequest();
  }else if(typeof ActiveXObject != 'undefined'){ // 兼容ie7-
    if(typeof arguments.callee.activeXString != 'string'){
      let versions = {
        'MSXML2.XMLHTTP.6.0',
        'MSXML2.XMLHttp.3.0',
        'MSXML2.XMLHttp'
      };

      for(let version of versions){
        try {
          new ActiveXObject(version);
          arguments.callee.activeXString = version;
          break;
        } catch {
          // 跳过
        }
      }
    }

    return new ActiveXObject(arguments.callee.activeXString);
  }else{
    throw new Error('没有可用的XHR对象.')
  }
}

```
- XHR用法
```js
let XHR = createXHR();

/**
 * readyState:
 * 0： 未初始化。尚未调用open()
 * 1： 启动。已经调用open()，但尚未调用send()方法
 * 2： 发送。已经调用send()方法，但尚未接收到响应
 * 3： 接收。已经接收到部分响应数据
 * 4： 完成。已经接收到全部响应数据，而且已经可以在客户端使用了
 *
 * status：
 * 200： OK
 * 404：未找到页面
 */
XHR.onreadystatechange = function(){
  if(XHR.readyState == 4){
    if((XHR.status >= 200 && XHR.status < 300) || XHR.status == 304){

    }else {
      //success
    }
  }else {
    // 请求失败
  }
};

XHR.open('get', 'example.txt', true);
XHR.send(null);

//XHR.abort()//终止请求

```
- HTTP头部信息
  + Accept：浏览器能够处理的内容类型
  + Accept-Charset：浏览器能够处理的字符集
  + Accept-Encoding：浏览器能够处理的压缩编码
  + Accept-Language：浏览器当前设置的语言
  + Connection：浏览器与服务器之间连接的类型
  + Cookie：当前页面设置的任何Cookie
  + Host：发出请求的页面所在域
  + Referer：发出请求的页面的URI
  + User-Agent：浏览器的用户代理字符串

- GET请求
  + 用于向服务器查询某些信息，必要时，可以将查询字符串参数追加到URL的末尾，以便将信息发送给服务器。对于XHR而言，位于传入open()法的URL末尾的查询字符串的每个参数的名称和值都必须使用encodeURIComponent()进行编码，然后才能放到URL末尾，而且所有的名-值对都必须由&分割
```js
let url = 'example.php'
url = addURLParam(url, 'name', 'dsagd')
url = addURLParam(url, 'value', 'dsagd')

XHR.open('get', url, true)


function addURLParam(url, name, value){
  url += (url.indexOf('?') == -1 ? '?' : '&');
  url += encodeURIComponent(name) + '=' + encodeURIComponent(value);
  return url;
}

```
- POST请求
  + 通常用于向服务器发送应该被保存的数据。post请求把数据作为请求的主体提交，请求的主体可以包含非常多的数据，而且格式不限
  ```
  // post请求必须在调用open()方法之后且调用send方法之前调用setRequestHeader()
  XHR.setRequestHeader('Content-type', 'application/x-www-form-urlencoded')
  ```
### XMLHttpRequest 2 级
- FormData
  + 用来解决现代web应用中频繁使用表单数据的序列化，不必明确的在XHR对象上设置请求头部。XHR对象能够识别传入的数据类型是FormData实例，并配置适当的头部信息。
```js
let data = new FormData();
data.append('name', 'nicho');
let XHR = createXHR();
XHR.onreadystatechange = function(){
  if(xhr.readyState == 4){
    if((XHR.status >= 200 && XHR.status < 300) || XHR.status == 304){
      // XHR.responseText
    }else {
      // 失败
    }
  }
}

XHR.open('post', 'postexample.php', true);
let form = document.getElementById('user-info');
XHR.send(new FormData(form))

```
- 超时设定
 + 表示请求在等待多少毫秒之后就终止。在给timeout设置一个数值后，如果在规定的时间内浏览器还没有接收到响应，那么就会触发timeout事件，进而会调用ontimeout事件处理程序
```js
let XHR = createXHR();
XHR.readystatechange = function(){
  if(XHR.readyState == 4){
    try(){
      if((XHR.status >= 200 && XHR.status < =300) || XHR.status == 304){
        // XHR.responseText
      }else {
        // 失败
      }
    }catch{
        //假设由ontimeout事件程序处理
    }
  }
}

XHR.open('get', 'timeout.php', true)
XHR.timeout = 1000; // 仅适用于ie8+
XHR.ontimeout = function(){
  // 请求没有立即返回。
}
XHR.send(null)
```
- overrideMimeType()
  - 用于重写XHR响应的MIME类型
  - 此方法必须在send()之前，才能保证重写响应的MIME类型
```js
let XHR = createXHR();
XHR.open('get', 'test.php', true);
XHR.overrideMimeType('text/xml'); // 强迫XHR对象将响应当做XML而非纯文本来处理
XHR.send(null);
```

### 进度事件
- 所有事件
  + loadstart：在接收到响应数据的第一个字节时触发
  + progress：在接收响应期间持续不断地触发
  + error：在请求发生错误时触发
  + abort：在因为调用abort()方法而终止连接时触发
  + load：在接收到完整的响应数据时触发
  + loadend：在通信完成或者触发error、abort或load事件后触发

- load事件
  + 只要浏览器接收到服务器的响应，不管其状态如何，都会触发load事件。
```js
let XHR = createXHR();
XHR.onload = function(){
  if((XHR.status >= 200 && XHR.status < 300) || XHR.status == 304){
    // XHR.responseText
  }else {
    // 失败
  }
};

XHR.open('get', 'test.php', true);
XHR.send(null);
```

- progress事件
  + 必须在调用open之前添加此事件
```js
let XHR = createXHR();
XHR.onload = function(event){
  if((XHR.status >= 200 && XHR.status < 300) || XHR.status == 304){
    // XHR.responseText
  }else {
    // 失败
  }
}

XHR.onprogress = function(event){
  let divStatus = document.getElementById('status');
  if(event.lengthComputable){ // 信息进度是否可用
    divStatus.innerHTML = '预期字节数' + event.totalSize + '，接收到' + event.position + '字节数'
  }
}

XHR.open('get', 'test.php', true)
XHR.send(null)
```
### 跨源资源共享（Cross-Origin Resource Sharing）
- IE对Cors的实现
  + IE引入了XDR（XDomainRequest），与XHR类似，但能实现安全可靠的跨域通信，
  + XDR与XHR的不同之处：
    * cookie不会随请求发送，也不会随响应返回
    * 只能设置请求头部信息中的Content-Type字段
    * 不能访问响应头部信息
    * 只支持GET和POST
    * open()只需两个参数：请求类型和URL
```js
let xdr = new XDomainRequest();
xdr.onload = function(){
  //xdr.responseText
}

xdr.onerror = function(){
  // 出错
}

xdr.timeout = 1000
xdr.ontimeout = function(){
  // 请求超时
}

// GET
//xdr.open('get', 'test/test/')
//xdr.send(null)


// POST
xdr.open('post', 'url')
xdr.contentType = 'applycation/x-www-from-urlencoded'
xdr.send('name1=value1&name2=value2')
```
- 其他浏览器对CORS的实现
  + 要请求位于另一个域中的资源，使用标准的XHR对象并且在open()方法中传入绝对URL即可
  + 与XDR不同，通过跨域XHR对象可以访问status和statusText属性，而且还支持同步请求，但为了安全，不能使用setRequestHeader()设置自定义头部，不能发送和接收cookie，调用getAllResponseHeaders()方法总会返回空字符串
```js
let xhr = createXHR();
xhr.onreadychange = function(){
  if(xhr.readyState == 4){
    if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
      // xhr.responseText
    }else {
      // 失败
    }
  }
}

xhr.open('get', 'http://url', true)
xhr.send(null)
```
- Preflighted Requeusts
  + 支持开发人员使用自定义的头部、GET或POST之外的方法，以及不同类型的主体内容。在使用下列高级选项来发送请求时，就会向服务器发送一个Preflight请求。这种请求使用OPTIONS方法，发送下列头部
    * Origin: 与简单的请求相同
    * Access-Control-Request-Method：请求自身使用的方法
    * Access-Control-Request-Headers：（可选），自定义的头部信息，多个头部以逗号分隔
  + 发送请求后，服务器可以决定是否允许这种类型的请求。服务器通过在响应中发送如下头部与浏览器进行沟通
    * Access-Control-Allow-Origin：与简单的请求相同
    * Access-COntrol-Allow-Methods：允许的方法，多个方法以逗号分隔
    * Access-Control-Allow-Headers：允许的头部，多个头部以逗号分隔
    * Access-Control-Max-Age：应该将这个Preflight请求缓存多长时间（以秒表示）
- 带凭据的请求
  + 通过将widthCredentials属性设置为true，可以指定某个请求应该发送凭据，如果服务器接受带凭据的请求，会用下面的HTTP头部来响应
    * Access-Control-Allow-Credients:true
  + 如果发送的是带凭据的请求，但服务器的响应中没有包含这个头部，那么浏览器就不会把响应交给Javascript，另外服务器还可以在Preflight响应中发送这个HTTP头部，表示允许源发送带凭据的请求
- 跨浏览器的CORS
```js
function createCORSRequest(method, url){
  let xhr = new XMLHttpRequest();
  if('WithCreadentials' in xhr){
    xhr.open(method, url, true)
  }else if(typeof XDomainRequest != 'undefined'){
    xhr = new XDomainRequest();
    xhr.open(method, url);
  }else {
    xhr = null;
  }

  return xhr;
}

let request = createCORSRequest('get', 'http://sdga');

if(request){
  request.onload = function(){
    // request.responseText
  }

  request.send()
}
```
### 其他跨域技术
- 图像Ping
  + 图像Ping是与服务器进行简单、单向的跨域通信的一种方式。请求的数据是通过查询字符串形式发送的，而响应可以是任意内容，但通常是像素图或204响应。通过图形Ping，浏览器得不到任何具体的数据，但通过侦听load和error事件，它能知道响应是什么时候接收到的
```js
let img = new Image();
img.onload = img.onerror = function(){
  alert('Done')
}
img.src = 'http://sdag'
```
- JSONP（json width padding）
  + 是通过动态是`<script>`标签来使用的，使用时可以为src属性指定一个跨域URL
  + 两部分组成：
    + 回调函数：是当响应到来时应该在页面中调用的函数
    + 数据：传入回调函数中的JSON数据
  + 缺点：
    * 从其他域加载代码执行，不安全
    * 要确定JSONP请求是否失败并不容易

```js
function handleResponse(data){

}
let script = document.createElement('script')
script.src = 'http://sagd?callback=handleResponse'
document.body.insertBefore(script, document.body.firstChild)
```
### 安全
- 为确保XHR访问的URL安全，通行的做法就是验证发送请求者是否有权限访问相应的资源
  + 要求以SSI连接访问可以通过XHR请求的资源
  + 要求每一次请求都要附带经过相应的算法计算得到的验证码
  + 要求发送POST而不是GET请求——很容易改变（对防范CSRF攻击不起作用）
  + 检查来源URL以确定是否可信——来源记录很容易伪造（对防范CSRF攻击不起作用）
  + 基于cookie信息进行验证——同样很容易伪造（对防范CSRF攻击不起作用）
