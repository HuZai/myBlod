---
title: nginx 配置vue项目 二级目录处理 
tags: vue
---
### 使用 lsof 查看端口进程
``` bash
lsof -i:[端口]
```
{% codeblock lang:js %}
bogon:nginx h5$ lsof -i:4000

COMMAND    PID  USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
    Google    3636   h5   30u  IPv6 0xed1a66db077ffd2d      0t0  TCP localhost:52494->localhost:terabase (CLOSE_WAIT)
    Google    3636   h5   32u  IPv6 0xed1a66db077ff76d      0t0  TCP localhost:52495->localhost:terabase (CLOSE_WAIT)
{% endcodeblock %}

### 杀掉端口进程
kill [pid]
``` bash
kill 3636
```
### nginx配置基础
#### try_files 指令使用说明
``` bash
try_files指令
语法：try_files file ... uri 或 try_files file ... = code
默认值：无
作用域：server location
exmple：
try_files $uri $uri/ /index.html;
```
实例说明
``` bash
 server {
            listen       8080;
            server_name  localhost;
            location /{
              try_files $uri $uri/ =404;
              index index.html index.htm;
           }
     }
如果访问地址是 http://localhost:8080/test
$uri 是指 test 会先去根目录下查找有没有 test文件 
如果没有匹配  $uri/ 找test 文件夹存不存在
如果还没有 404
```

### vue 相关设置
1.router 根路由设置 'my' 
``` bash
export default new Router({
  mode: 'history',
  base:'/my/',
  routes: [
    ...
  ]
```
2.打包配置 publicPath 
```bash
module.exports = {
  publicPath: process.env.NODE_ENV === 'production' ? '/my/' : '/',
      ...
}
```
3.nginx 对应配置
``` bash
 server {
            listen       8080;
            server_name  localhost;
            location /{
               try_files $uri $uri/ /my/index.html;
              index index.html index.htm;
           }
     }
vue 打包dist 目录里的文件
上传到nginx 根目的 my 文件下
```