### 文件读写操作
```shell
// 引入文件模块
var fs = require('fs');
// 读取文件(同目录下的a.txt)
fs.readFile('./a.txt', 'utf8', function (err, data) {
  console.log('err', err);
  console.log('data', data.toString());
});
```

```shell
var fs = require('fs');
// 写入文件
fs.writeFile('.b.txt', 'hello node', function (err, data) {
  console.log('err', err);
  console.log('data', data);
});
```

### http模块
node的http模块继承自net，借助http模块，几行代码就可以搞定一个简单的服务器
```shell
var http = require('http');

// 创建http server
var server = http.createServer(function(serverReq, serverRes) {
  var url = serverReq.url;
  serverRes.end('hello node 您访问的地址是', url);
});
server.listne(3000);

// http client 实例
var client = http.get('http://127.0.0.1:3000', function(clientRes){
  clientRes.pipe(process.stdout);
});

// http 向页面写入内容
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

### url模块
小tips，学习node的知识，可以在控制台中查找，例如url模块，可以在node中输入 url 可以得到所有的方法
