---
title: vue cli3 多页面配置 history 路由配置处理
tags:
	- vue
---

#### 1.多页面配置 vue.config.js 里配置pages [vue cli多页面配置文档](https://cli.vuejs.org/zh/config/#pages)

    module.exports = {
      pages: {
        index: {
          // page 的入口
          entry: 'src/index/main.js',
          // 模板来源
          template: 'public/index.html',
          // 在 dist/index.html 的输出
          filename: 'index.html',
          // 当使用 title 选项时，
          // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
          title: 'Index Page',
          // 在这个页面中包含的块，默认情况下会包含
          // 提取出来的通用 chunk 和 vendor chunk。
          chunks: ['chunk-vendors', 'chunk-common', 'index']
        },
        // 当使用只有入口的字符串格式时，
        // 模板会被推导为 `public/subpage.html`
        // 并且如果找不到的话，就回退到 `public/index.html`。
        // 输出文件名会被推导为 `subpage.html`。
        subpage: 'src/subpage/main.js'
      }
    }
#### 2.history 路由需要增加以下配置 否则 subpage入口下的路由访问不到处理
如果subpage 入口路由都是以/sub/开头

则增加一下配置即可

所有以/sub/开头的路由 都会指向 subpage.html

    module.exports={
      ...
      devServer:{
        historyApiFallback: {
          rewrites: [
            { from: /^\/sub/, to: '/subpage.html' },
          ]
        }
      },
      ...
     }
    
#### 3.项目地址

[github demo](https://github.com/HuZai/multi-page)
