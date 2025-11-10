### 启动后端
本地配置数据库与redis启动时首先需要将`IS_LOCAL_MODE`设置为`false`
```js
{
    "scripts": {
    "build": "cross-env node ./scripts/replaceToRelativePath.js && tsc -p ./tsconfigForBuild.json && node ./scripts/copy-views.js",
    "start": "cross-env IS_LOCAL_MODE=false IS_SIMPLE_SERVER=0 NODE_PATH=./build nodemon -e ts ./build/index.js",
    "start:simple": "cross-env IS_LOCAL_MODE=true NODE_PATH=./build nodemon -e ts ./build/index.js",
    "[[start hint]]": "your must execute build before execute start"
  }
}
```
#### 数据库配置
数据库配置文件在 `helpack\server\src\at\configs\env\index.ts`中
```js
{
    ...
    dbConf: {
    username: '用户名',
    password: '密码',
    database: '数据库名称',
    host: '主机地址',
    port: '端口号',
    dialect: 'mysql',
  } as unknown as nsdb.DBConfigItem,
  ...
}
```
在server目录下有一键建表脚本`helpack\server\db.sql` 在命令行中链接你设置的数据库，执行
```bash
cd server 
mysql -u 你的用户名 -p 你的密码 你的数据库名称
source db.sql
```
即可一键完成建表工作
- tips:初次进入helpack平台默认用户名为`hi-bro`需要在`t_user_info`表中添加用户信息

#### redis配置
redis配置文件在 `helpack\server\src\at\configs\env\index.ts`中
```js
{
    ...
    redisConf: {
    ip: 'IP地址',
    port: 端口号,
    password: '密码',
    keyPrefix: 'xc_',
  },
  ...
}
```
#### 启动
当为本地配置数据库和redis启动时，启动后端命令如下
```bash
$ cd server
$ npm i
$ npm run build        // 先编译ts文件
$ npm run start        // 再启动编译好的工程
```
成功启动后可见端口号（默认7777端口）打开浏览器访问 http://localhost:7777

> 推荐配置vscode的launch.json文件，方便快速启动，配置如下
```js
{
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "debug-js",
      "program": "${workspaceFolder}/server/build/index.js",
      "env": {
        "NODE_PATH": "${workspaceFolder}/server/build/",
        "IS_LOCAL_MODE": "true"
      }
    },
    {
      "name": "debug",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/server/src/index.ts",
      "outFiles": ["${workspaceFolder}/server/build/**/*.js"],
      "env": {
        "NODE_PATH": "${workspaceFolder}/server/build",
        "IS_LOCAL_MODE": "true"
      }
    }
  ]
}
```
### 启动前端
```bash
cd client
npm i
npm start
```
### 前端部署
**部署到本地server**
处于前端项目根目录`client`下时，执行
``` bash
npm run build:local
```
会自动清理后端public目录后，再将其构建产物复制到那里