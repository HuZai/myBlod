---
title: vue 源码__[断点调试]
tags:
	- vue
---
{% blockquote %}
  可以debug vue源码 有助于快速理解vue源码
{% endblockquote %}
##### 1.clone Vue 项目到本地 安装nodemodules
    git clone https://github.com/vuejs/vue
    
    npm install
##### 2.修改 vue package.json  文件 增加--sourcemap
    ...
    "scripts": {
        "dev": "rollup -w -c scripts/config.js --environment TARGET:web-full-dev --sourcemap", 
        }
    ...    
##### 3. 执行 npm run dev 
    在 dist 目录下回新增一个 vue.js.map 文件
##### 4. 引入 dist/vue.js
    <script src="../../dist/vue.js"></script> 
    或者修改 examples 任意一个例子
     <script src="../../dist/vue.min.js"></script>
     为<script src="../../dist/vue.js"></script>
##### 5 访问demo调试
    比如：
    http://localhost:63342/vue-dev/examples/commits/index.html
   【注】
    访问demo需要 HTTP 服务器
    可以使用一个 Node.js 静态文件服务器，例如 [serve](https://github.com/zeit/serve) 
    
   [serve具体使用点这里](https://cli.vuejs.org/zh/guide/deployment.html#%E9%80%9A%E7%94%A8%E6%8C%87%E5%8D%97)
    
     