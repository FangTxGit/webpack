### Webpack

###### 简介

webpack 是一个用于现代 JavaScript 应用程序的 静态模块打包工具。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图(dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。

###### 生成环境配置

- css
- html 模版页
- less
- 图片
- 抽离 css
- postcss => postcss postcss-loader posscss-preset-env

```
 package.json 中配置 postcss 获取寻找 开发环境和生产环境的配置
   "browserslist": {
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ],
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ]
  }
```

- webpack-dev-server
- babel => babel-loader @babel/core @babel/preset-env core-js
- ts 语法支持
- externals
- resolve => alias - extensions

###### 开发环境配置

- css 压缩 => css-minimizer-webpack-plugin

###### 优化配置

- HMR 和 Sourcemap
- oneOf

```
    oneOf 只能匹配一次loader 比如ts进来 匹配到ts之后就不在继续匹配了 用于优化
    如果有相同的loader 比如 js 需要2组loader配置的话 就不能写到这里
    enforce 执行顺序
          pre 优先处理
          normal 正常处理（默认）
          inline 其次处理
          post 最后处理
```

- 缓存

```
 babel 缓存使用  cacheDirectory cacheCompression
 项目缓存 使用 hash chunkhash contenthash
```

- tree-shaking

```
   tree-shaking 作用 优化代码打包体积， 比如js 文件有add sml 2个方法 如果只用了add 那么该文件只有add方法被打包 sml放弃丢弃
   tree-shaking开启条件  1 比如是 esModule 引入    2  必须是生产环境 mode:'production'
```

- 代码分割 codesplit

```
   1 可以采用 多页面默认 进行的代码分割 entry :{ a:xxxx,b:xxxxx}
   2 可以使用 optimization :{ splitChunks :{ chunks:'all'}} 将 node_modules 中的文件 单独打包
   3 如果是项目内部的页面 可以使用 import(/webpackChunkName:'test'/,'./test').then((result)=>{ //此处使用文件中的方法 }).catch(()=>{})
```

- 懒加载和预加载

```

  懒加载技术 ：
  import(/* webpackChunkName:'test1' , webpackPrefetch:true /'./test').then(({ add }) => { add() }).catch(() => { })
  在页面使用import引入是懒加载 返回promise类型
  例如点击按钮之后 执行import 才去加载文件
  预加载 webpackPrefetch:true 使用预加载技术 浏览器会在页面开始就加载文件 点击按钮之后 会加载页面的缓存文件速度较快
  预加载 浏览器上来就加载文件 是在其它资源加载完成以后 在加载的 不会单独页面主要流程的进度，属于偷偷加载～～

```

- 多进程打包

```
    loader: thread-loader 开启多进程打包
    如果项目很小运行很快的话不需要开启
    因为thread-loader 启动也需要时间大概600ms 进程通信也需要时间 默认开启线程数是 cpu数量 - 1
    所以一般给比较耗时的loader使用 比如babel-loader
    如果是大型项目可以考虑开启
```

- Dll

```
  DLL 动态连接库
  作用： 可以单独告诉 webpack 那些包 不需要打包，例如 :jquery ，单独打 jquery 的包 并且简历 manifest.json 文件的映射关系
  这样只要 jquery 不更新 我们就不需要在打包 jquery 每次都通过 manifest.json 文件 映射出来就好 可以减少打包体积
  操作：需要单独写配置文件 webpack.dll.js 名字随意 生成单独的 jquery 文件和 manifest 文件
  webpack.config.js 中 引入映射关系即可
  使用的插件 有 webpack.DllPlugin webpack.DllReferencePlugin
  通过 add-asset-html-webpack-plugin 将文件引入到 html
```

- PWA 渐进式网络开发应用程序
  ![Alt](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/5/24/1639200a468412e2~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)

```
  /**
   * PWA 技术  渐进式网络开发应用程序 （离线访问技术 serverWorker）
   * 下载包 workbox-webpack-plugin
   * 加载plugin
   *    new WorkboxWebpackPlugin.GenerateSW({
       * 1 帮助serverWork快速启动
       * 2 删除旧的serverWork文件
          skipWaiting: true,
          clientsClaim: true
        })
   * 页面入口文件写入serivceWorker的注册事件
   * if ('serviceWorker' in navigator) {
        window.addEventListener('load', () => {
          navigator.serviceWorker.register('/service-worker.js').then(() => {
            console.log('注册成功了');
          }).catch(() => {
            console.log('注册失败了');
          })
        })
      }
   */
```
