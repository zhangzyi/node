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

fs流及其读取
```shell
const fs = require('fs');
// 流的方式读取文件
let fileReadStream = fs.createReadStream('index.js');
// 读取次数
let count = 0;
// 保存数据
let str = '';
// 开始读取
fileReadStream.on('data', (chunk) => {
  console.log(`${++count} 接收到：${chunk.length}`);
  // Console：1 接收到：30
  str += chunk;
})
// 读取完成
fileReadStream.on('end', () => {
  console.log("——结束——");
  console.log(count);
  console.log(str);

  // Console：——结束——
  // 1
  // console.log("Hello World！");
})
// 读取失败
fileReadStream.on('error', (error) => {
  console.log(error);
})
```

### http模块
node的http模块继承自net，借助http模块，几行代码就可以搞定一个简单的服务器

一个最简单的例子

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

// 综合利用node的http模块，url模块，path模块，fs模块创建一个服务器，读取本地html、css、js文件，将内容输出到浏览器
```shell
var http = require('http');
var fs = require('fs');
http.createServer((req, res) => {

  let pathName = req.url;
  // 获取文件的后缀名
  let extName = path.extname(pathName);

  fs.readFile('./index.html', (err, htmlData) => {
    if (err) {
      fs.readFile('./404not_found.html', (err, dataNotFound) => {
        if (!err) {
           res.writeHead(200, {
              "Content-Type": "text/html; charset='utf-8'"
            });
            // 读取写入文件
            res.write(dataNotFound);
            // 结束响应
            res.end();
      })
    } else {
       // 获取文件类型
      let ext = getExt(extName);
      res.writeHead(200, {
        "Content-Type": ext +"; charset='utf-8'"
      });
      // 读取写入文件
      res.write(dataNotFound);
      // 结束响应
      res.end();
    } 
  });
}).listen(8080);

// 获取后缀名
getExt = (extName) => {
  switch(extName) {
    case '.html': return 'text/html';
    case '.css': return 'text/css';
    case '.js': return 'text/js';
    default: return 'text/html';
  }
}
```



### url模块
小tips，学习node的知识，可以在控制台中查找，例如url模块，可以在node中输入 url 可以得到所有的方法
```shell
console.log(url);

/**
 * Console：
 { 
   Url: [Function: Url],
    parse: [Function: urlParse], // 获取地址信息
    resolve: [Function: urlResolve], // 追加或者替换地址
    resolveObject: [Function: urlResolveObject],
    format: [Function: urlFormat], // 逆向 parse，根据地址信息获取原 url 信息
    URL: [Function: URL],
    URLSearchParams: [Function: URLSearchParams],
    domainToASCII: [Function: domainToASCII],
    domainToUnicode: [Function: domainToUnicode] 
  }
 */

```

```shell
// 一个小的🌰
var url = require("url");
var http = require("http");

http.createServer(function(req, res) {
  var result = url.parse(req.url, true);
  console.log(result);
  // 设置 HTTP 头部，状态码是 200，文件类型是 html，字符集是 utf8
  res.writeHead(200, {
    "Content-Type": "text/html;charset=UTF-8"
  });

  // 往页面打印值
  res.write('<h1 style="text-align:center">Hello NodeJS</h1>');
  // 结束响应
  res.end();
}).listen(3000);
```

### 


