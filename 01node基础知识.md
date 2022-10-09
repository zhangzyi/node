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