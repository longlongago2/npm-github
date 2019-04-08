# npm+github

依托 github+npm 开发和维护前端 lib 模块

## 背景介绍

> 前端模块化开发大潮：以前的开发模式，代码冗长混杂，组件复用困难，即使提取出可复用插件，往往都是要通过 script 引入js包，往往插件或方法又都暴露在全局，引入太多，往往会带来大概两个问题：<br><br>
1、没有命名空间，会污染全局变量，<br>
2、如果我的插件又依赖别人的插件，别人的插件又依赖别人的插件，如此循环，又会面临处理复杂的依赖逻辑关系。<br>
从利用 CommonJs，requireJs 将前端模块化（解决第一个痛点），再到npm，webpack等自动处理复杂的依赖关系（解决第二个痛点），全面的模块化开发打包全流程管理，从青涩一步步走向成熟，这个过程也衍生出了很多规范！<br><br>
> 衍生规范：<a href="#1">CommonJs(node.js)</a> --> <a href="#2">AMD(require.js)</a> --> <a href="#3">CMD(seaJS)</a> --> <a href="#4">UMD(兼容 CommonJs, AMD, var)</a><br><br>
一系列的规范产生和发展，最终形成了今天的格局。随着模块化的深度使用，也让我们探索到底什么样的情况适合作为一个lib输出。<br><br>
这里列举几种我个人的想法：<br>
1、与主业务逻辑无关，但又包含很多操作逻辑的富应用，如富文本编辑器之类的<br>
2、可以提取出来的公共UI组件库，例如element，iview，ant-design等等，这些通用组件都是可以通过传入属性或参数去实例化符合要求的不同形态<br>
3、utils纯工具库，例如moment，lodash<br>
。。。

注：

<a name="1">
CommonJs规范： 它是服务端模块化加载规范，由Node推广使用，只能依赖node环境执行。纳尼？？？这么好的东西，是不是就对传统前端开发来说没用了呢？其实，webpack就是使用这个规范去打包依赖，因为 webpack 本身就是依赖 node 环境开发出来的服务端预处理工具。标志性语法：require('module-name') / module.exports，这个 require 和 AMD 的 require 不同，下面会讲到！
</a><br><br>

<a name="2">
requireJs(AMD 规范)：如果说 CommonJs 的出现解决了服务端模块化的问题，那么 require.js(AMD 规范)的出现就是主要解决客户端模块化的问题。（？？？解释上面留的疑问）那么它与 CommonJs 的 require 具体又有什么关系呢？应该说他们什么关系都没有，唯一可能的关系就是传统前端工程师看见 node 工程师可以使用 commonjs 模块化加载眼红，大家都用 js，为什么你行，我不行，所以就发明出来一种适合客户端模块化加载的规范，CommonJs 依赖的是 node 环境的加载，而 requireJs 就是纯客户端浏览器加载的一种规范，它可以直接在浏览器执行，而 commonjs 就不行（但是可以用于 webpack 预处理工具打包构建之后，就可以在浏览器运行啦）。
</a><br><br>

<a name="3">
seaJs(CMD 规范)：结合了 CommonJs 的理念和 AMD 的原理的客户端模块化加载规范，CMD 特别就在于，体现了 CommonJs 按需加载的理念，摒弃了 AMD 的依赖前置，这是一种过渡态，用的不多，具体怎么体现的语法可以自己了解。
</a><br><br>

<a name="4">
UMD 规范：首先 UMD 是什么，中文解释就是通用模块定义，兼容 AMD，CommonJS 和全局变量，一统天下。使用 UMD 规范之后，各种规范自动向下兼容。简单来说，良心的 lib 都应该用 umd 规范发布，这样，node 环境；模块化环境（webapck 等预处理工具、requireJs 等）；传统 CDN 环境（全局变量），就都可以使用啦。那么这么好的东西又该怎么玩呢？webpack 针对 lib 模式（webpack 打包有两种模式，runtime 模式和 library 模式：我们平常使用的生产环境的打包，就是 runtime 模式，但是 webpack 还可以针对模块化进行打包，即 lib 模式） 打包时提供了这个选择，以下是原文：

```
output.libraryTarget
Which format to export the library:
"var" - Export by setting a variable: var Library = xxx (default)
"this" - Export by setting a property of this: this["Library"] = xxx
"commonjs" - Export by setting a property of exports: exports["Library"] = xxx
"commonjs2" - Export by setting module.exports: module.exports = xxx
"amd" - Export to AMD (optionally named)
"umd" - Export to AMD, CommonJS2 or as property in root
```

</a><br><br>

## 名词介绍

### 1. Node

`node:` Node.js® 是一个基于 Chrome V8 引擎 的 JavaScript 运行时。[官网](https://nodejs.org/zh-cn/)

- 怎么理解：

- JavaScript 之前只是一种运行在客户端浏览器中的脚本语言，它只能以浏览器 webview 作为宿主执行，而不能像 java 一样直接操作于服务端，而 nodejs 的作用就是可以在服务端执行 JavaScript，让它具备像 java 一样的服务端语言的功能。

- 后端的同事可以理解 node 的 v8 引擎它就类似于 jvm，是一种可以执行 JavaScript 的运行环境。

### 2. Npm

`npm:` NPM 是随同 NodeJS 一起安装的一款非常实用的包管理工具。[官方搜索网站](https://www.npmjs.com/)

- 主要作用：

- 用于安装和管理依赖包。 npm install 安装，npm run [script] 执行命令行等等

- 后端的同事可以把 npm 想象成一个简易版的 maven

### 3. Webpack

`webpack:` 本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包工具。[官网](https://webpack.docschina.org/)

- 主要功能：

- loader: 用于对模块的源代码进行转换，对 html js css 等进行预处理。loader 可以将文件从不同的语言（如 Es6 转换成 Es5；TypeScript 转换为 JavaScript；将 inline-image 转换为 data URL；less 转换成 css...），loader 甚至允许你直接在 JavaScript 模块中 import CSS 文件！

- plugin: 插件是 webpack 的支柱功能。插件目的在于解决 loader 无法实现的其他事，例如热更新 Hot Module Replacement(HMR：开发时只需保存代码不用手动刷新直接更新页面)。

- devServer: 能够用于快速开发应用程序，启动本地 localhost 服务，利用 proxy 反向代理解决接口跨域等

- 后端的同事可以把它想象成 tomcat（不完全准确）。

### 4. Eslint

`eslint:` 可配置的 JavaScript 和 JSX 代码检测工具。 [官网](https://cn.eslint.org/docs/user-guide/configuring)

- 主要功能：

- 方便统一 js 代码规范和写法，可配置规则对代码进行报错或警告提示，利用插件自动修复等。

- 后端同事可以理解为类似于 java CheckStyle 静态代码检查工具。

### 5. Markdown

`markdown:` Markdown 是一门比 html 更简单的标记语言，其主要用于书写工程项目 readme 文档。 [官网](http://www.markdown.cn/) / [基础语法](https://www.jianshu.com/p/191d1e21f7ed)

### 6. Typescript

`typescript:` 是 JavaScript 类型的超集，本质上是对 js 添加了可选的静态类型和基于类的面向对象编程。

- `d.ts:` 一个纯粹的类型声明文件，不涉及逻辑。

- d.ts 作用：

- 一个好用的 library 插件智能提示是不可或缺的。因为 lib 插件本身就是安装在 node_modules 模块中，我们很少去查看它的源码，因此使用时文档和智能提示就显得非常重要

- d.ts 就经常用于增强 IDE 智能提示。javascript 本身是弱类型，例如参数类型常常不明确，对 IDE 的提示就不是很友好，而 ts 却可以明确定义类型，如果我的代码使用纯 js 书写，却想让他拥有和 ts 一样的类型智能提示，我们就可以书写声明文件 _\*.d.ts_ 。[官方文档](https://www.tslang.cn/docs/handbook/declaration-files/introduction.html) / [\*.d.ts 代码示例](https://github.com/longlongago2/react-native-sqlite-helper/blob/master/sqliteHelper.d.ts)

### 7. Jest

`jest:` 前端单元测试框架。 [官网](https://jestjs.io/zh-Hans/) / [代码示例](https://github.com/longlongago2/react-native-sqlite-helper/blob/master/_tests_/sqliteHelper.test.js)，其他常用测试框架还有[Mocha](https://mochajs.org/)

- 使用场景：

- 执行测试脚本，执行完毕后提供精准的测试报告，报错代码覆盖率，有效避免人肉测试导致的场景逻辑遗漏。

- 在项目正在进行的情况下，随时进行单元测试，不用等待整体完工或者有具体可操作的界面的时候再测试。

![jest](./assets/jest.png)

![jest](./assets/jest2.png)

![jest](./assets/jest3.png)

### 8. Git

`git:` Git 是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理

### 9. Github

`github:` 就是本网站。

- 简单介绍：

- GayHub 是全球最大的同性交友平台。~，其实 GitHub 是一个面向开源及私有软件项目的托管平台，因为只支持 git 作为唯一的版本库格式进行托管，故名 GitHub。

- 不过，为什么说它是交友网站呢？你打开 Github 上一个用户的主页，他的头像 (avatar)、喜欢的人 (following) 、喜欢他的人 (followers)、个人能力 (repositories)、活跃度 (contributions) ，一清二楚...

![github](./assets/github.jpg)

_注：这是旧版，被微软收购后新版已改版_

### 10. Gitlab

`gitlab:` 不用介绍了，它是仅次于 GitHub 的同性交友平台。

## 开发流程

> <a href="#1-1">新建远程仓库并克隆到本地</a> --> <a href="#1-2">初始化项目</a> --> <a href="#1-3">配置脚手架</a> --> <a href="#1-4">组件开发</a> --> <a href="#1-5">单元测试</a> --> <a href="#1-6">配置声明文件</a> --> <a href="#1-7">撰写readme文档</a> --> <a href="#1-8">打包发布</a>

### <a name="1-1">1. 新建远程仓库并克隆到本地</a>

`Create a new repository`

`git clone [url]`

### <a name="1-2">2. 初始化项目</a>

`npm init`

### <a name="1-3">3. 配置脚手架</a>

`npx webpack-cli init`

### <a name="1-4">4. 组件开发</a>


### <a name="1-5">5. 单元测试</a>


### <a name="1-6">6. 配置声明文件</a>


### <a name="1-7">7. 撰写readme文档</a>


### <a name="1-8">8. 打包发布</a>



## 其他好玩的事儿

### 1. 徽章

如果你有逛 GitHub 的习惯，应该就已经发现，许多知名的开源项目的 README 上都有着花花绿绿的各式徽章。比如说这个：

![徽章](./assets/badge.png)

那么这些徽章都是干啥的呢？ [玩转 github 徽章](https://www.jianshu.com/p/afba6e33e48e) / [自己定制徽章](https://shields.io/)

单纯的链接

[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)

使用证书

[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)

[![LICENSE](https://img.shields.io/github/license/longlongago2/npm-github.svg)](./LICENSE)

npm 版本

[![LICENSE](https://img.shields.io/npm/v/npm-github.svg)](./package.json)

聊天室 Gitter

[![LICENSE](https://img.shields.io/gitter/room/longlongago2/npm-github.svg)](./package.json)

Travis 使用.travis.yml 配置文件，告诉 Travis CI 怎样对你的项目进行编译或测试，成功徽章会自动展示 passing

[![LICENSE](https://img.shields.io/travis/com/longlongago2/npm-github.svg)](./package.json)

Codacy 编程代码自动审查服务平台，可以帮我们分析存在的问题 或者说是 bug,主要包括代码质量、语法规范、功能可用性方面的检查。

[![LICENSE](https://img.shields.io/codacy/coverage/npm-github.svg)](./package.json)

订阅数

[![LICENSE](https://img.shields.io/github/stars/longlongago2/react-native-sqlite-helper.svg?style=social)](https://github.com/longlongago2/react-native-sqlite-helper)

... 还有很多，喜欢的自己探索自己探索

### 2. 开源许可证

常用的几种证书

![证书](./assets/license.png)
