---
title: vue cli3 +ts + pwa 配置详解
date: 2019-08-11 15:21:51
tags:
	- vue
	- TypeScript
	- postcss
---
---
    使用 vue cli3 搭建项目 
    vue create hello-world 
    按空格健 选择以下 选项
         ◉ Babel
         ◉ TypeScript
         ◉ Progressive Web App (PWA) Support
         ◉ Router
         ◉ Vuex
         ◉ CSS Pre-processors  
         ◉ Linter / Formatter
         ◉ Unit Testing
---
### TypeScript相关处理
---
    执行安装
    npm i -S vue-class-component vue-property-decorator vuex-class
    
---
#### 1. tsconfig.json 配置 [配置详解](https://www.tslang.cn/docs/handbook/compiler-options.html)
    {
      "compilerOptions": {
        "target": "esnext",
        "module": "esnext",
        "strict": true,
        "jsx": "preserve",
        "importHelpers": true,
        "moduleResolution": "node",
        "experimentalDecorators": true,
        "esModuleInterop": true,
        "allowSyntheticDefaultImports": true,
        "sourceMap": true,
        "baseUrl": ".",
        "types": [
          "webpack-env"
        ],
        "paths": {
          "@/*": [
            "src/*"
          ]
        },
        "lib": [
          "esnext",
          "dom",
          "dom.iterable",
          "scripthost"
        ]
      },
      "include": [
        "src/**/*.ts",
        "src/**/*.tsx",
        "src/**/*.vue",
        "tests/**/*.ts",
        "tests/**/*.tsx"
      ],
      "exclude": [
        "node_modules"
      ]
    }

#### 2. vue-property-decorator vue官方维护 类修饰器支持（typescript）  [more](https://github.com/kaorun343/vue-property-decorator)
    
    vue-property-decorator 依赖vue-class-component 
    语法
    @Component({
      props: {
        propMessage: String
      }
    })
    export default class App extends Vue {
      // initial data
      msg = 123
    
      // use prop values for initial data
      helloMsg = 'Hello, ' + this.propMessage
    
      // lifecycle hook
      mounted () {
        this.greet()
      }
    
      // computed
      get computedMsg () {
        return 'computed ' + this.msg
      }
    
      // method
      greet () {
        alert('greeting: ' + this.msg)
      }
    }
    
[更多例子](https://github.com/vuejs/vue-class-component)   
#### 3. vuex-class vuex修饰器[more](https://github.com/ktsn/vuex-class/)
    import Vue from 'vue'
    import Component from 'vue-class-component'
    import {
      State,
      Getter,
      Action,
      Mutation,
      namespace
    } from 'vuex-class'
    
    const someModule = namespace('path/to/module')
    
    @Component
    export class MyComp extends Vue {
      @State('foo') stateFoo
      @State(state => state.bar) stateBar
      @Getter('foo') getterFoo
      @Action('foo') actionFoo
      @Mutation('foo') mutationFoo
      @someModule.Getter('foo') moduleGetterFoo
    
      // If the argument is omitted, use the property name
      // for each state/getter/action/mutation type
      @State foo
      @Getter bar
      @Action baz
      @Mutation qux
    
      created () {
        this.stateFoo // -> store.state.foo
        this.stateBar // -> store.state.bar
        this.getterFoo // -> store.getters.foo
        this.actionFoo({ value: true }) // -> store.dispatch('foo', { value: true })
        this.mutationFoo({ value: true }) // -> store.commit('foo', { value: true })
        this.moduleGetterFoo // -> store.getters['path/to/module/foo']
      }
    }
#### 4. 设置声明文件 第三方未提供可用的声明文件 处理如下
    a. 使用vuedraggable
    在XXX.d.ts文件里添加一下 vuedraggable 就可以正常使用了
    
    declare module 'vuedraggable'
    
    
    b. axios 使用 XXX.d.ts 添加如下
    
    import Vue from 'vue'
    declare module "vue/types/vue" {
      interface Vue {
        $https: any
      }
    }

#### 5. 使用了vue-class-component   vue-router 路由守卫处理
     // class-component-hooks.js
     import Component from 'vue-class-component'
     Component.registerHooks([
       'beforeRouteEnter',
       'beforeRouteLeave',
       'beforeRouteUpdate' // for vue-router 2.2+
     ])
     
### PWA相关处理 vue.config.js 里 以GenerateSW 为例
    ...
    pwa: {
        name: 'My App1',
        themeColor: '#4DBA87',
        msTileColor: '#000000',
        appleMobileWebAppCapable: 'yes',
        appleMobileWebAppStatusBarStyle: 'black',
        workboxPluginMode: 'GenerateSW',
        // workboxPluginMode: 'InjectManifest',
        workboxOptions: {
          runtimeCaching: [
            {
              urlPattern: /leaf/,
              handler: 'networkFirst',
              options: {
                networkTimeoutSeconds: 10,
                cacheName: 'my-api-cache',
                expiration: {
                  maxEntries: 5,
                  maxAgeSeconds: 30 // 30 days
                },
                cacheableResponse: {
                  statuses: [200]
                }
              }
            },
            {
              urlPattern: /.*\.(?:js|css)/,
              handler: 'networkFirst',
              options: {
                cacheName: 'my-js-cache',
                expiration: {
                  maxEntries: 30,
                  maxAgeSeconds: 30
                },
                cacheableResponse: {
                  statuses: [0, 200]
                }
              }
            }
          ]
        }
      },
      ...
 [更多相关配置](https://github.com/vuejs/vue-docs-zh-cn/tree/master/vue-cli-plugin-pwa)
 [runtimeCaching 相关配置](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin)
 ['networkFirst'等缓存策略见我另一片文章](/2019/08/09/pwa/)