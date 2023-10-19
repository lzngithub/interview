# 工程化

## 1. 谈谈你对 WebPack 的认识。

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。可以将前端资源（如 JavaScript、CSS、图片等）作为模块处理，根据模块的依赖关系进行静态分析，打包生成对应的静态资源。

webpack 有五个核心概念：

- entry：入口，指示 webpack 应该使用哪个模块，来作为构建其内部 依赖图(dependency graph) 的开始。
- output：输出，告诉 webpack 在哪里输出它所创建的 bundle，以及如何命名这些文件。
- loader: webpack 只能理解 JavaScript 和 JSON 文件，loader 让 webpack 能够去处理其他类型的文件。
- plugin: 插件,功能增强，用于执行范围更广的任务，如：打包优化、资源管理，注入环境变量。
- mode: 模式，分为开发环境和生产环境，通过设置 mode，可以启用 webpack 内置在相应环境下的优化。

作用：

1. 模块打包
2. 编译兼容（loader）
3. 功能扩展（plugin）

## 2. WebPack 的核心原理是什么？

Webpack 的核心原理主要基于以下几个概念：

- 模块化：Webpack 将所有资源（如 JavaScript、CSS、图片等）视为模块，每个模块都可以使用 import 或 require 语法来引入其他模块。这样，我们可以更方便地管理代码的依赖关系，提高代码的可维护性和可重用性。
- 依赖图：Webpack 通过分析每个模块的依赖关系，构建出一个完整的依赖图。这个依赖图描述了各个模块之间的依赖关系，Webpack 会根据这个依赖图来确定模块之间的执行顺序。
- 入口和出口：在 Webpack 中，我们可以设置一个或多个入口文件，这些文件是构建依赖图的起点。Webpack 会从入口文件开始，逐级分析各个模块的依赖关系，构建出一个完整的依赖图。然后，Webpack 会根据这个依赖图，将各个模块打包成一个或多个输出文件，这些输出文件就是我们可以在浏览器中直接运行的代码。
- 插件：插件是 Webpack 生态中最关键的部分。它们为社区用户提供了一种强有力的方式来直接触及 Webpack 的编译过程。插件可以用来实现各种功能，如代码压缩、合并、分割等，极大地扩展了 Webpack 的功能。

总的来说，Webpack 的核心原理就是将所有的资源视为模块，通过分析模块之间的依赖关系，构建出一个完整的依赖图。然后，根据这个依赖图，将各个模块打包成一个或多个输出文件。在这个过程中，插件可以用来实现各种功能，帮助我们更好地管理和优化代码。

## 3. 说说 WabPack 打包的流程。

1. 读取 webpack 的配置参数；
2. 启动 webpack，创建 Compiler 对象并开始解析项目；
3. 从入口文件（entry）开始解析，并且找到其导入的依赖模块，递归遍历分析，形成依赖关系树；
4. 对不同文件类型的依赖模块文件使用对应的 Loader 进行编译，最终转为 Javascript 文件；
5. 整个过程中 webpack 会通过发布订阅模式，向外抛出一些 hooks，而 webpack 的插件即可通过监听这些关键的事件节点，执行插件任务进而达到干预输出结果的目的。

## 谈谈对 loader 的理解？

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

即文件加载器：用来预处理文件。操作的是文件，将文件 A 通过 loader 转换成文件 B，是一个单纯的文件转化过程。

- 处理一个文件可以使用多个 loader，loader 的执行顺序和配置中的顺序是相反的，最后一个 loader 最先执行，第一个 loader 最后执行。
- 第一个执行的 loader 接受源文件作为参数内容，其他的 loader 接受前一个 loader 的返回值作为自己的参数，最后一个执行的 loader 会返回此模块的 JavaScript 源码。
- loader 本质上是一个 ESM 模块，它导出一个函数，在函数中对打包资源进行转换
- loader 必须返回一段 js 代码，这样 eval 才能执行，否则会报错，多个 loader，保证最后面一个返回 js 代码就行。

## 谈谈对 plugin 的理解？

plugin 即插件：插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

即本身是一个扩展器，用来增强功能。针对的是在 loader 结束之后，webpack 打包的整个过程，他并不直接操作文件，而是基于事件机制工作，监听 webpack 打包过程中的某些节点，执行广泛的任务。

特性：

- 是一个独立的模块
- 模块对外暴露一个 js 函数
- 函数的原型 (prototype) 上定义了一个注入 compiler 对象的 apply 方法
- apply 函数中需要有通过 compiler 对象挂载的 webpack 事件钩子，钩子的回调中能拿到当前编译的 compilation 对象，如果是异步编译插件的话可以拿到回调 callback
- 完成自定义子编译流程并处理 complition 对象的内部数据
- 如果异步编译插件的话，数据处理完成后执行 callback 回调
- loader 运行在打包文件之前，plugins 在整个编译周期都起作用

## 常用的 webpack loader 有哪些？

- style-loader：用于将 css 编译完成的样式，挂载到页面 style 标签上。
- css-loader: 用于识别.css 文件, 处理 css 必须配合 style-loader 共同使用，只安装 css-loader 样式不会生效。
- sass-loader： css 预处理器，识别.scss 文件。
- postcss-loader: 用于补充 css 样式各种浏览器内核前缀，太方便了，不用我们手动写啦。
- babel-loader： 将 Es6+ 语法转换为 Es5 语法。
- ts-loader： 用于配置项目 typescript。
- html-loader：我们有时候想引入一个 html 页面代码片段赋值给 DOM 元素内容使用，这时就用到 html-loader。
- file-loader: 用于处理文件类型资源，如 jpg，png 等图片。返回值为 publicPath 为准。
- url-loader: url-loader 也是处理图片类型资源，只不过它与 file-loader 有一点不同，url-loader 可以设置一个根据图片大小进行不同的操作，如果该图片大小大于指定的大小，则将图片进行打包资源，否则将图片转换为 base64 字符串合并到 js 文件里。
- html-withimg-loader：我们在编译图片时，都是使用 file-loader 和 url-loader，这两个 loader 都是查找 js 文件里的相关图片资源，但是 html 里面的文件不会查找所以我们 html 里的图片也想打包进去，这时使用 html-withimg-loader
- vue-loader：用于编译.vue 文件，如我们自己搭建 vue 项目就可以使用 vue-loader。
- eslint-loader: 用于检查代码是否符合规范，是否存在语法错误。

## 常用的 webpack plugin 有哪些？

- HtmlWebpackPlugin: 根据模板文件自动生成 html 文件，并且将输出文件 JS 自动插入到 html 中，免去了需要手动更新版本号的烦恼。
- CommonsChunkPlugin：抽取公共模块，减小包占用的内存空间，例如 vue 的源码、 jQuery 的源码等。
- CleanWebpackPlugin: 用于清除打包的输出文件夹，试想如果每次打包出的文件都带有不同版本号，不及时清除文件夹，那么是不是会越来越大。本插件可以帮助我们自动清除，避免手动操作。
- CopyWebpackPlugin: 将文件从某一目录复制到另一目录，可以用于 public 下的静态文件，比如和 html 同级的 JS 文件，无需参与编译但是需要输出到 dist 目录
- webpack.DefinePlugin：定义某些变量的值，在打包时用变量值替代变量。
- MiniCssExtractPlugin：抽离 css 到独立的文件中，否则将会以内嵌的方式嵌在 html 中。
- PurgeCSSPlugin: 删除没有引用到的选择器及其样式，需要配置判断引用文件，如 html，js。起到 css tree-shaking 的效果。需要注意的是，不会删除重复的选择器样式，这个属于压缩的任务。
- webpack.ProvidePlugin：配置全局模块，避免多次引入的麻烦。
- BundleAnalyzerPlugin：文件分析插件，可以用于打包后资源的依赖及大小分析。
- ImageminPlugin：压缩图片
- CompressionPlugin：启用传输压缩，比如将资源压缩为 GZIP 格式。需要服务端进行配合。
- webpack.NoEmitOnErrorsPlugin：遇到编译报错不输出。比如我们启用热加载开发时，改错资源引用将导致页面实时报错，配置该插件可以让遇到错误的编译不再输出资源文件，页面也不会更新报错。打包时也是如此，遇到错误将跳过输出。
- OptimizeCssAssetsPlugin：压缩 css，会去除重复的类名样式。
- UglifyJsPlugin：压缩 JS。
- webpack.HotModuleReplacementPlugin：热加载插件，配合热加载设置，提高开发效率。
- TerserPlugin：压缩 JS，和 UglifyJsPlugin 插件相比，能更好的处理 ES6 以上语法。
- optimization.splitChunks：提取多入口的公共文件，避免重复打包。或者提取某一模块文件（如 node_modules），配合页面缓存来提高页面加载速度。

## 如何优化 Webpack 的构建速度？

1. 使用高版本的 webpack 和 node.js
2. 使用更快的硬件：例如使用更快的 CPU 或者将项目部署在更快的服务器上。
3. 多进程/多实例构建：thread-loader，在 webpack 构建过程中，实际上耗费时间大多数在 loader 解析转换以及代码的压缩中等，由于 js 单线程的特性使得这些转换操作不能并发处理文件，一个个文件进行处理，而使用 HappyPack 可以开启多进程 Loader 转换。
4. 缩小搜索范围：在 Webpack 打包时，如果你的代码中 import 或 require 了别的模块或第三方库，Webpack 会按照一定的规则去搜索这些模块。可以通过设置 alias、include、exclude、noParse 来缩小 Webpack 的搜索范围，从而加快打包速度。
5. 开启缓存：对于 Babel-loader 等支持缓存转换结果的插件，可以通过 cacheDirectory 选项开启缓存，避免重复转换。
6. 使用 DLL：使用 DLL 可以提前将一些不常变动的代码和资源打包成一个 dll 文件，然后在构建项目的时候直接引用这个 dll 文件，从而避免了重复打包，提高了构建速度。
7. 提取页面公共资源：基础包分离：使用 html-webpack-externals-plugin，将基础包通过 CDN 引入，不打入 bundle 中。

## webpack 优化产出代码

- 小图片 base64 编码
- 提取公共代码
- bundle 加 hash
- 使用 CDN 加速
- 懒加载
- IgnorePlugin
- 使用 production
- Scope Hosting

## ES6 Module 和 Commonjs 区别

- ES6 Module 静态引入，编译时引入
- Commonjs 动态引入，执行时引入
- 只有 ES6 Module 才能静态分析，实现 Tree-Shaking

## 如何为项目创建 package. json 文件？

运行 npm init

## publicPath 是什么？

在 WebPack 自动生成资源路径时会参考 publicPath 参数，如果文件是与页面放到一起的，那么可以按相对路径来设置，比如'./'之类的；而如果 JavaScript、CSS 文件用于存放 CDN，当然就要填写 CDN 的域名和路径。

## 当使用 html- webpack- plugin 时找不到指定的 template 文件怎么办？

也就是将以前的 file-loader 修改为 html- loader 就可以了。

## 图片处理常见的加载器有几种？

1. file-loader，默认情况下会根据图片生成对应的 MD5 散列的文件格式。
2. url-loader，它类似于 file- loader，但是 url- loader 可以根据自身文件的大小，来决定是否把转化为 base64 格式的 DataUrl 单独作为文件，也可以自定义对应的散列文件名。
3. image-webpack-loader，提供压缩图片的功能。

## webpack 如何解决开发时的跨域问题？

- 在开发时，我们的页面在 localhost:8080，JS 直接访问后端接口 https://yunmu.com，为了解决这个问题，可以在 webpack.config.js 中添加开发服务器代理配置:

```js
module.exports = {
  //...

  devServer: {
    // 请求 /api/users 就会自动被代理到 http://xiedaimala.com/api/users
    proxy: {
      '/api': {
        target: 'http://yunmu.com',
        // 使请求中的 Origin 从 8080 修改为 xiedaimala.com
        changeOrigin: true,
        // 不配置证书且忽略 HTTPS 报错
        secure: false,
      },
    },
  },
};
```

## tree-shaking 的原理是什么？

Tree-shaking 翻译过来的意思就是摇树。这棵树可以把它比喻为现实中的树，可以这样理解，摇树就是把发黄、没有作用还要汲取养分的叶子给给摇掉。把这个搬到 javascript 程序里面来就是，移除 Javascript 上下文中的未引用代码（dead-code）。

1. 解析源代码生成 ast（抽象语法树）
2. 遍历 ast，记录相关信息
3. 根据遍历 ast 得到的信息，生成新代码

## swc、esbuild 是什么？

- ESBuild/swc 是用编译型语言编写的新一代前端工具, 对 JS 编写的构建工具有系统级的速度优势
- ESBuild 可以用于编译 JS 代码和模块打包, swc 号称也都可以支持两者但是其打包工具还处于早期开发阶段
- 目前这两个工具还不能完全替代 Webpack 等主流工具这些年发展出的庞大生态
- 当已有的基础设施稳定并且替换成本较大时, 可以尝试渐进式的利用新工具(loader)或者 Vite 这种基于 ESBuild 二次封装的构建工具

swc

- 实现语言：Rust
- 功能：编译 JS/TS、打包 JS/TS
- 优势：比 babel 快很多很多很多（20 倍以上）
- 能否集成进 webpack：能
- 使用者：Next.js、Parcel、Deno、Vercel、ByteDance、Tencent、Shopify……
- 做不到：对 TS 代码进行类型检查（用 tsc 可以）、打包 CSS、SVG

esbuild

- 实现语言：Go
- 功能：编译 JS/TS、打包 JS/TS
- 优势：比 babel 快很多很多很多很多很多很多（10~100 倍）
- 能否集成进 webpack：能
- 使用者：vite、vuepress、snowpack、umijs、blitz.js 等
- 做不到：对 TS 代码进行类型检查、打包 CSS、SVG

## express.js VS koa.js 对比

- 中间件模型不同：express 的中间件模型为线型，而 koa 的为 U 型（洋葱模型）
- 对异步的处理不同：express 通过回调函数处理异步，而 koa 通过 generator 和 async/await 使用同步的写法来处理异步，后者更易维护，但彼时 Node.js 对 async 的兼容性和优化并不够好，所以没有流行起来
- 功能不同：express 包含路由、渲染等特性，而 koa 只有 http 模块

## nodejs 多进程

- 进程，是操作系统进行资源调度和分配的基本单位，每个进程都拥有自己独立的内存区域
- 线程，是操作系统进行运算调度的最小单位，一个进程可以包含多个线程，多线程之间可共用进程的内存数据
- 一个进程无法直接访问另一个进程的内存数据，除非通过合法的进程通讯
- 执行一个 nodejs 文件，即开启了一个进程，可以通过 process.pid 查看进程 id
- 如操作系统是一个工厂，进程就是一个车间，线程就是一个一个的工人
- JS 是单线程的，即执行 JS 时启动一个进程（如 JS 引擎，nodejs 等），然后其中再开启一个线程来执行
- 虽然单线程，JS 是基于事件驱动的，它不会阻塞执行，适合高并发的场景

## 为何需要多进程

- 现代服务器都是多核 CPU ，适合同时处理多进程。即，一个进程无法充分利用 CPU 性能，进程数要等于 CPU 核数
- 服务器一般内存比较大，但操作系统对于一个进程的内存分配是有上限的（2G），所以多进程才能充分利用服务器内存

## nodejs 开启多进程

- child_process.fork 可开启子进程执行单独的计算
  - fork('xxx.js') 开启一个子进程
  - 使用 send 发送信息，使用 on 接收信息
- cluster.fork 可针对当前代码，开启多个进程来执行

## 扩展：使用 PM2

- nodejs 服务开启多进程、进程守护，可使用 pm2
- 全局安装 pm2 yarn global add pm2
- 增加 pm2 配置文件
- 修改 package.json scripts

参考网址：

- 常用的 loader：https://juejin.cn/post/6942322281913778206
- 常用的 plugin：https://juejin.cn/post/6906089118447435784
