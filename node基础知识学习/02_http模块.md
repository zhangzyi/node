#### http模块概览

在node.js中, http是最核心的模块, 只需要几行代码，就可以启动一个本地服务

#### 几行代码创建一个 web server

最简单的一个小🌰
```shell
var http = require('http');

// http server 例子
var server = http.createServer(function(serverReq, serverRes){
    var url = serverReq.url;
    serverRes.end( '您访问的地址是：' + url );
});

server.listen(3000);

// http client 例子
var client = http.get('http://127.0.0.1:3000', function(clientRes){
  clientRes.pipe(process.stdout);
});
```

再来一个🌰，向页面中写入内容
```shell
http.createServer(function (req, res) {
  // 设置 HTTP 头部，状态码是 200，文件类型是 html，字符集是 utf8
  res.writeHead(200, {
    "Content-Type": "text/html;charset=UTF-8"
  });

  // 往页面打印值
  res.write('<h1 style="text-align:center">Hello NodeJS</h1>');

  // 结束响应，访问localhost:3001
  res.end();
}).listen(3001); // 监听的端口
```

#### req 对象

req对象，其实是http.IncomingMessage的一个实例
在服务端：获取请求对的相关信息，如：request、header
在客户端：获取响应方法的相关信息，如statusCode

服务端的🌰
```shell
// 下面的 req
var http = require('http');
var server = http.createServer(function(req, res){
    console.log(req.headers);
    res.end('ok');
});
server.listen(3000);
```

客户端的🌰
```shell
// 下面的res
var http = require('http');
http.get('http://127.0.0.1:3000', function(res){
    console.log(res.statusCode);
});
```

服务端获取 httpVersion/method/url/get/post请求参数🌰


```shell
// getClientInfo.js
var http = require('http');

var server = http.createServer(function(req, res){
  console.log( '1、客户端请求url：' + req.url );
  console.log( '2、http版本：' + req.httpVersion );
  console.log( '3、http请求方法：' + req.method );
  console.log( '4、http请求头部' + JSON.stringify(req.headers) );

  res.end('ok');
});

server.listen(3000);
```

获取get请求参数
```shell
// getClientGetQuery.js
var http = require('http');
var url = require('url');
var querystring = require('querystring');

var server = http.createServer(function(req, res){
  var urlObj = url.parse(req.url);
  var query = urlObj.query;
  var queryObj = querystring.parse(query);
  
  console.log( JSON.stringify(queryObj) );
  
  res.end('ok');
});

server.listen(3000);
```

获取post请求参数
```shell
// getClientPostBody.js
var http = require('http');
var url = require('url');
var querystring = require('querystring');

var server = http.createServer(function(req, res){
    
  var body = '';  
  req.on('data', function(thunk){
      body += thunk;
  });

  req.on('end', function(){
      console.log( 'post body is: ' + body );
      res.end('ok');
  }); 
});

server.listen(3000);
```

客户端获取httpVersion/statusCode/statusMessage🌰

```shell
var http = require('http');
var server = http.createServer(function(req, res){
  res.writeHead(200, {'content-type': 'text/plain'});
  res.end('from server');
});
server.listen(3000);

var client = http.get('http://127.0.0.1:3000', function(res){
  console.log('1、http版本：' + res.httpVersion);
  console.log('2、返回状态码：' + res.statusCode);
  console.log('3、返回状态描述信息：' + res.statusMessage);
  console.log('4、返回正文：');

  res.pipe(process.stdout);
});
```

#### res对象
res的职责是，向客户端返回正确的相应内容，包括：状态代码/状态描述信息、响应头部、响应主体

一个基础的🌰
```shell
var http = require('http');

// 设置状态码、状态描述信息、响应主体
var server = http.createServer(function(req, res){
  res.writeHead(200, 'ok', {
      'Content-Type': 'text/plain'
  });
  res.end('hello');
});

server.listen(3000);
```

也可以
```shell
res.writeHead(200, 'ok');
```

```shell
res.statusCode = 200;
res.statusMessage = 'ok';
```

```shell
res.setHeader('Content-Type', 'text-plain');
```

设置相应主体、超时处理
response.write(chunk[, encoding][, callback])
response.end([data][, encoding][, callback])
response.setTimeout(msecs, callback)

chunk：响应主体的内容，可以是string，也可以是buffer。当为string时，encoding参数用来指明编码方式。（默认是utf8）
encoding：编码方式，默认是 utf8。






