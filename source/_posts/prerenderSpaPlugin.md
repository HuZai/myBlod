---
title: vue cli3 结合 prerender-spa-plugin 预渲染做seo 配置 
tags: vue
---
遇到问题：

1.项目路由设置了 base 后prerender-spa-plugin打包生成的静态文件是白页

原因是：Prerender-spa-plugin在dist目录中启动了一个服务。 资源的引用路径不对 导致无头浏览器 抓取不到页面 

网上资料都是一些简单例子，所以整理下我的配置处理

## 所有配置如下

### 一. vue-router 设置

``` bash
new Router({
  mode: 'history',
  base:'/seo/',
  ...
  })
  mode 设置 history
  base 基路径 
```
### 二. vue.config.js vue基本设置 设置打包文件路径
``` bash
module.exports = {
  publicPath: process.env.NODE_ENV === 'production'
      ? '/seo/'
      : '/',
  outputDir:'dist/seo',
  ...
}
outputDir 默认 是'dist'
```
More info: [vue cli 配置](https://cli.vuejs.org/zh/config/#publicpath)
### 三. prerender-spa-plugin相关配置
``` bash
const PrerenderSPAPlugin = require('prerender-spa-plugin')
const Renderer = PrerenderSPAPlugin.PuppeteerRenderer
const path = require('path')
module.exports = {
  publicPath: process.env.NODE_ENV === 'production'
      ? '/seo/'
      : '/',
  outputDir:'dist/seo',
  chainWebpack: config => {
      config.plugin('PrerenderSPAPlugin').use(PrerenderSPAPlugin, [
        {
          staticDir: path.join(__dirname, 'dist'),
          outputDir: path.join(__dirname, 'dist/seo'), //打包生成问文件
          indexPath: path.join(__dirname, 'dist', '/seo/index.html'), //首页访问路径
          routes: ['/','/about'], //要预渲染的路由
          renderer: new Renderer({
            inject: {
              foo: 'bar'
            },
            renderAfterTime: 5000,  //renderAfterTime与 renderAfterDocumentEvent  同时起作用 哪个快执行哪个
            headless: false,
            // 在 main.js 中 document.dispatchEvent(new Event('render-event'))，两者的事件名称要对应上。
            // renderAfterDocumentEvent: 'render-event'
          })
        }
      ])
    }
    ...
}
```
More info: [vue cli 链式操作](https://cli.vuejs.org/zh/guide/webpack.html#链式操作-高级)
### 四. 打包后静态文件 nginx 相关配置
打包后文件目录
```bash
dist
└── seo
    ├── about
    │   └── index.html
    ├── css
    │   └── app.cdd9559b.css
    ├── favicon.ico
    ├── img
    │   ├── card1.d9965849.png
    │   ├── icons
    │   └── logo.82b9c7a5.png
    ├── index.html
    ├── js
    │   ├── app.1f058936.js
    │   ├── app.1f058936.js.map
    │   ├── chunk-vendors.5d7c9b8d.js
    │   └── chunk-vendors.5d7c9b8d.js.map
    ├── manifest.json
    ├── precache-manifest.5dafe8fe95e326b40f63cb53925f6a27.js
    ├── robots.txt
    └── service-worker.js
```
nginx相关配置 把 seo文件夹放到nginx 根目录
``` bash
server {
       listen       8080;
       server_name  localhost;
       location /seo/{
          try_files $uri $uri/ /seo/index.html;
          index index.html index.htm;
       }
     }
```