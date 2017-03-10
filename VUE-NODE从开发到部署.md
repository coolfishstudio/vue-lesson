# vue-node 从开发到部署
搭建一个基于node/express的vue单页项目
对于这个项目首先应该理解为前后端应该分离，
客户端由vue开发静态页面，vue-router做路由，后端提供一个API服务，以及静态服务。

## 正式开始
### 1.安装Vue-Cli
```
npm install vue-cli -g
```
### 2.创建一个项目文件，按提示一直下一步，完成后按提示
```
vue init webpack vue-node-demo
```
### 3.安装包并运行
```
cd vue-node-demo
npm install
npm run dev
```
### 4.修改结构
- 将`src`文件夹修改为`client`
- 将`webpack.base.conf.js`内的`src`地址修改为`client`

### 5.创建服务端
```
mkdir server
cd server/
touch app.js
```
### 6.安装express
```
npm install express --save
npm install body-parser --save
```
### 7.写后端启动文件 app.js
```
var express = require('express');
var fs = require('fs');
var path = require('path');
var bodyParser = require('body-parser');

var app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
// 访问静态资源
app.use(express.static(path.resolve(__dirname, '../dist')));
// 访问单页
app.get('*', function (req, res) {
    var html = fs.readFileSync(path.resolve(__dirname, '../dist/index.html'), 'utf-8');
    res.send(html);
});
// 监听
app.listen(9900, function () {
    console.log('success listen...9900');
});

```
后期配合业务进行修改逻辑
### 打包并部署
```
npm run build
node server/app.js
```
在浏览器输入`http://127.0.0.1:9900/`就能看到页面了

如此 就完成了我认为的前后端在一个项目里分离各自开发到正式部署的流程了。
