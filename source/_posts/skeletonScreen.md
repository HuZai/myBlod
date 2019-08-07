---
title: 首屏展示优化 骨架屏 vue
tags: vue
---
`
骨架屏概念已经很久了，实现骨架屏方式有很多种。下面介绍几种好用的骨架屏实现方案
`
## page-skeleton-webpack-plugin 自动生成骨架屏 由ElemeFE团队开发

### 1.配置如下 基于vue cli3

``` bash
const { SkeletonPlugin } = require('page-skeleton-webpack-plugin')
module.exports = {
    chainWebpack: config => {
        config.plugin('html').tap(opts => {
            opts[0].minify = { removeComments: false }
            return opts
          })
        config.plugin('SkeletonPlugin').use(SkeletonPlugin, [
          {
            pathname: path.resolve(__dirname, `./shell`), // 用来存储 shell 文件的地址
            staticDir: path.resolve(__dirname, './dist'), // 最好和 `output.path` 相同
            routes: ['/', '/about'] // 将需要生成骨架屏的路由添加到数组中
          }
        ])
      },
    }
```
More info: [vue cli 链式操作](https://cli.vuejs.org/zh/guide/webpack.html#链式操作-高级)

`shell 目录是用来存取生成的骨架屏目录。生成操作见：下面page-skeleton-webpack-plugin配置使用文档`

**vue cli3 下`page-skeleton server listen at port: 8989 `报这个错误 按下修改**
```$xslt
page-skeleton-webpack-plugin 包 下src/skeletonPlugin.js 25行 修改如下

SkeletonPlugin.prototype.createServer = function () { // eslint-disable-line func-names
  if (!this.server) {
    const server = this.server = new Server(this.options) // eslint-disable-line no-multi-assign
    server.listen().catch(err => server.log.warn(err))
  }
}
```
More info: [page-skeleton-webpack-plugin有个fix分支未合并](https://github.com/ElemeFE/page-skeleton-webpack-plugin/commit/cd6e14af157bbee9d3442e7b5fd0df79c2b43ce3)

More info: [page-skeleton-webpack-plugin配置使用文档](https://github.com/ElemeFE/page-skeleton-webpack-plugin/blob/master/docs/i18n/zh_cn.md)
### 2.html 里设置  `<!-- shell -->` 插件会替换此标签为对应的骨架屏

``` bash
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title>eventcmswww</title>
  </head>
  <body>
    <div id="app">
      <!-- shell -->
    </div>
    <!-- built files will be auto injected -->
  </body>
</html>
```
遇到的问题build后骨架屏没有出现

`  原因cli3 自动打开了打包注视删除操作。按上面设置removeComments false`
打包后文件路径如下
```$xslt
    dist
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
#### 使用感受

    1.会自动生成对应的静态文件 但是其他没有骨架屏的页面进入 会加载默认的骨架屏 此处不友好
    2.骨架屏展示 到消失的原理：
        a.会根据对应的路由生成对应的html 文件
        b.浏览器加载对应的页面在 js加载之前首先渲染静态dom骨架屏展示出现
        c.页面里js加载完成后 加载对应的路由 替换index.html 里 <div id=app ></div> 中间的内容骨架屏消失
        
        
## vue-skeleton-webpack-plugin 根据已有骨架屏自动插入对应的路由
### 1.vue.config.js配置
```$xslt
const SkeletonWebpackPlugin = require('vue-skeleton-webpack-plugin')
const path = require('path')
module.exports = {
    chainWebpack: config => {
        config.plugin('SkeletonWebpackPlugin').use(SkeletonWebpackPlugin,[
              {
                webpackConfig: {
                  entry:{
                    app: path.resolve(__dirname,'./src/entry-skeleton.js')
                  }
                },
                quiet: true,
                minimize: true,
                router: {
                  mode: 'history',  //router 模式
                  routes: [
                    {
                      path: '/seo',
                      skeletonId: 'skeletonIndex'
                    },
                    {
                      path: '/seo/about',
                      skeletonId: 'skeletonAbout'
                    },
                  ]
                }
              }
            ])
    }
}
```
### 2.entry-skeleton.js 文件设置
```$xslt
import Vue from 'vue';
import Skeleton1 from './skeleton/Skeleton1.vue';
import Skeleton2 from './skeleton/Skeleton2.vue';

export default new Vue({
    components: {
        Skeleton1,
        Skeleton2
    },
    template: `
        <div>
            <skeleton1 id="skeletonIndex" style="display:none"/>
            <skeleton2 id="skeletonAbout" style="display:none"/>
        </div>
    `
});

```
    注：
        1.id[skeletonIndex] 与vue.config.js里的skeletonId 想对应
        2.entry-skeleton.js 里引入的Skeleton1.vue 文件就是骨架屏文件
#### 使用感受  
    1.此插件会把骨架屏文件根据配置打包到 index.html里 通过路由path与skeletonId 来匹配展示
    2.可以对骨架屏做路由 开发环境下调试开发
    3.需要手写骨架屏文件  比较麻烦
## vue-content-loader 快速自定义出骨架屏 react-content-loader（react）
    一个快速开发 绚丽骨架屏的框架
    
More info: [vue-content-loader](https://github.com/egoist/vue-content-loader)

More info: [基于vue-content-loader在线设置骨架屏](http://danilowoz.com/create-vue-content-loader/)