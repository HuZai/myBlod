---
title: vue项目 vw 适配px自动转vw 配置以及 兼容处理
date: 2019-08-06 15:21:51
tags:
	- vue
	- postcss
---
---
    使用postcss相关插件:
    postcss-px-to-viewport 做px 转 vw
    postcss-viewport-units与viewport-units-buggyfill 做低版本兼容处理
    其他postcss相关插件 对写css的友好帮助
    
---

`下面看用到的所有 postcss插件`
 1.[postcss-import](https://github.com/postcss/postcss-import)
 2.[postcss-url](https://github.com/postcss/postcss-url)
 3.[autoprefixer](https://github.com/postcss/autoprefixer)
 4.[postcss-aspect-ratio-mini](https://github.com/yisibl/postcss-aspect-ratio-mini)
 5.[~~postcss-cssnext~~](https://github.com/MoOx/postcss-cssnext) || [postcss-preset-env](https://github.com/csstools/postcss-preset-env)
 6.[postcss-px-to-viewport](https://github.com/evrone/postcss-px-to-viewport)
 7.[postcss-write-svg](https://github.com/jonathantneal/postcss-write-svg)
 8.[cssnano](https://github.com/ben-eb/cssnano)
 9.[Viewport Units Buggyfill](https://github.com/rodneyrehm/viewport-units-buggyfill)
 10.[postcss-viewport-units](https://github.com/springuper/postcss-viewport-units)

### 1.postcss-import [相关配置更多文档](https://github.com/postcss/postcss-import)
    功能是解决@import引入路径繁琐问题。
    它可以让你很轻易的使用本地文件、web_modules或者node_modules的文件。
    它一般可以结合postcss-url 使用更加容易。
    如下使用：
    /* can consume `node_modules`, `web_modules` or local modules */
    @import "cssrecipes-defaults"; /* 等价与 @import "../node_modules/cssrecipes-defaults/index.css"; */
    @import "normalize.css"; /* 等价与 @import "../node_modules/normalize.css/normalize.css"; */
    @import "foo.css"; /* relative to css/ according to `from` option above */
    @import "bar.css" (min-width: 25em);
    body {
      background: black;
    }
    相当于引入
    /* ... content of ../node_modules/cssrecipes-defaults/index.css */
    /* ... content of ../node_modules/normalize.css/normalize.css */
    /* ... content of css/foo.css */
    @media (min-width: 25em) {
    /* ... content of css/bar.css  bar.css里 min-width下面的css*/
    }
    body {
      background: black;
    }
### 2.postcss-url [相关配置更多文档](https://github.com/postcss/postcss-url)
      主要用来处理文件，比如图片文件、字体文件等引用路径的处理。
      vue项目里 Vue Loader 拥有相关功能所以 postcss-url 不需要做特殊设置
   [Vue Loader相关详细配置](https://vue-loader.vuejs.org/zh/guide/asset-url.html#%E8%BD%AC%E6%8D%A2%E8%A7%84%E5%88%99)
### 3.autoprefixer [相关配置更多文档](https://github.com/postcss/autoprefixer)
    它是用来自动处理浏览器前缀的一个插件
    它使用browserslist 目标浏览器配置 来处理浏览器兼容性前缀
    
    before
    ::placeholder {
      color: gray;
    }
    
    .image {
      background-image: url(image@1x.png);
    }
    @media (min-resolution: 2dppx) {
      .image {
        background-image: url(image@2x.png);
      }
    }
    after
    ::-webkit-input-placeholder {
      color: gray;
    }
    ::-moz-placeholder {
      color: gray;
    }
    :-ms-input-placeholder {
      color: gray;
    }
    ::-ms-input-placeholder {
      color: gray;
    }
    ::placeholder {
      color: gray;
    }
    
    .image {
      background-image: url(image@1x.png);
    }
    @media (-webkit-min-device-pixel-ratio: 2),
           (-o-min-device-pixel-ratio: 2/1),
           (min-resolution: 2dppx) {
      .image {
        background-image: url(image@2x.png);
      }
    }

---
    以上三个插件 vue cli 默认安装了
    默认配置
    {
        ...
        "autoprefixer": {},
        "postcss-import": {},
        "postcss-url": {},
        ...
    }
    共用browserslist配置的 插件有
    Autoprefixer
    Babel
    postcss-preset-env
    eslint-plugin-compat
    stylelint-no-unsupported-browser-features
    postcss-normalize
    obsolete-webpack-plugin
 [browserslist详细配置](https://github.com/browserslist/browserslist)
 
 ### 4.postcss-aspect-ratio-mini [相关配置更多文档](https://github.com/yisibl/postcss-aspect-ratio-mini)
     用来处理宽高比的
     Input
     .aspect-box {
         position: relative;
     }
     
     .aspect-box {
         aspect-ratio: '16:9';
     }
     Output
     .aspect-box {
         position: relative;
     }
     
     .aspect-box:before {
         padding-top: 56.25%;
     }
     
具体使用例子 添加公共样式

    [aspectratio] {
      position: relative;
    }
    [aspectratio]::before {
      content: '';
      display: block;
      width: 1px;
      margin-left: -1px;
      height: 0;
    }
    [aspectratio-content] {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      width: 100%;
      height: 100%;
    }

具体使用html 如下: （如果[w-375-100]还想加其他的css 属性比如background 需要跟aspect-ratio 分开单独另写。否则background 丢失）
    
    <div aspectratio w-375-100>
      <div aspectratio-content>content</div>
    </div>
    <style>
        [w-375-100] {
          aspect-ratio: '375:100';
        }
        [w-375-100] {
              background: #eeeeee;
        }
    </style>
### 5.postcss-preset-env [相关配置更多文档](https://moox.io/blog/deprecating-cssnext/)
    postcss-preset-env 取代了postcss-cssnext 
    postcss-preset-env比postcss-cssnext功能更强大
    postcss-preset-env 实质是 cssnext 让我们使用CSS未来的特性
    例子
    :root {
      --mainColor: red;
    }
    
    a {
      color: var(--mainColor);
    }
[更多cssnext特性](https://cssnext.github.io/features/)
### 6.postcss-px-to-viewport [相关配置更多文档](https://github.com/evrone/postcss-px-to-viewport)
    常用配置如下
    "postcss-px-to-viewport": {
            "viewportWidth": 375,   // 视窗的宽度，对应的是我们设计稿的宽度
            "unitPrecision": 5,     //指定`px`转换为视窗单位值的小数位数
            "viewportUnit": "vw",   // 指定需要转换成的视窗单位，建议使用vw
            "selectorBlackList": [  // 指定不转换为视窗单位的类，可以自定义，可以无限添加,
              ".ignore",
              ".hairlines"
            ],
            "minPixelValue": 1,   // 小于或等于`1px`不转换为视窗单位 可随意设置
            "mediaQuery": false     //是否允许在媒体查询中转换`px`
          },
     
     px 转换原则
     100vw=375px 1vw=3.75px
    
### 7.postcss-write-svg [相关配置更多文档](https://github.com/jonathantneal/postcss-write-svg)
      它主要用来处理移动端1px的解决方案
      input
      @svg square {
      	@rect {
      		fill: var(--color, black);
      		width: 100%;
      		height: 100%;
      	}
      }
      
      #example {
      	background: white svg(square param(--color #00b1ff));
      }
      output
      #example {
      	background: white url("data:image/svg+xml;charset=utf-8,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Crect fill='%2300b1ff' width='100%25' height='100%25'/%3E%3C/svg%3E");
      }
      
 [在线使用postcss-write-svg ](https://jonneal.dev/postcss-write-svg/)
 
### 8.cssnano [相关配置更多文档](https://github.com/ben-eb/cssnano)
      它主要是对css 进行压缩清理对CSS代码执行转换优化
      配置如下 使用 cssnano advanced设置 
      "cssnano": {
              "preset": [
                "advanced",
                {
                  "autoprefixer": false,
                  "zindex": false
                }
              ]
            }
      关闭autoprefixer vue默认安装了 autoprefixer 
      关闭 zIndex 否则 css zIndex 可能会都变成1
 [cssnano详细优化配置](https://cssnano.co/guides/optimisations/)
### 9.Viewport Units Buggyfill [相关配置更多文档](https://github.com/rodneyrehm/viewport-units-buggyfill)
    var hacks = require('viewport-units-buggyfill/.hacks');
    require('viewport-units-buggyfill').init({
      hacks: hacks
    });
    它需要你的CSS中，只要使用到了viewport的单位（vw、vh、vmin或vmax ）地方，都需添加content：
    例：
    .test {
        width: 50vw;
        height: 50vw;
        /* hack to engage viewport-units-buggyfill */
        content: 'viewport-units-buggyfill;  width: 50vw;height: 50vw;'
    }
    手动添加太麻烦 因此用到了下面插件postcss-viewport-units 自动添加content
### 10.postcss-viewport-units [相关配置更多文档](https://github.com/springuper/postcss-viewport-units)
    配置:
     "postcss-viewport-units": {
            "silence": true
          },
### 下面说一下 安装与所有配置
安装postcss 相关插件
```
npm install postcss-preset-env postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg  cssnano -S
```
  
安装vw 兼容插件  
   `npm install postcss-viewport-units viewport-units-buggyfill -S`
   
#### 相关所有配置 pageage.json
    ...
    "postcss": {
        "plugins": {
          "autoprefixer": {},
          "postcss-import": {},
          "postcss-url": {},
          "postcss-aspect-ratio-mini": {},
          "postcss-write-svg": {
            "utf8": false
          },
          "postcss-px-to-viewport": {
            "viewportWidth": 375,
            "unitPrecision": 5,
            "viewportUnit": "vw",
            "selectorBlackList": [
              ".ignore",
              ".hairlines"
            ],
            "minPixelValue": 1,
            "mediaQuery": false
          },
          "postcss-viewport-units": {
            "silence": true
          },
          "cssnano": {
            "preset": [
              "advanced",
              {
                "autoprefixer": false,
                "zindex": false
              }
            ]
          }
        }
      },
      "browserslist": [
        "> 1%",
        "last 2 versions"
      ],
      ...