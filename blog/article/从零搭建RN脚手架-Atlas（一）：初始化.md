### 从零搭建RN脚手架-Atlas（一）：初始化

#### 前言

大家好哦！接触 [React Native](https://github.com/facebook/react-native) 差不多一年时间了，期间经历过大大小小好几个项目。和大部分人一样，最开始只是用官方脚手架生成一个简单的项目，跑跑demo还行，应用到公司项目中，还是有一些乏力的。很多第三方库要自己手动去引入，特别是遇到周期短的项目，着实有些慌张。

经过一段时间的技术沉淀，我们的新项目基本是由 [Ignite](https://github.com/infinitered/ignite) 去生成。Ignite帮我们配置了很多有用的第三方库，直接上手撸项目也没有什么大问题（我们也确实是这样去做的）。但是毕竟很多工作是由它帮我们完成的，所以项目中很多地方的技术点，我们都只知其然，不知其所以然。

为了让自己对整个项目的架构有更深入的理解以及掌控力，我觉得自己有必要去做一套属于自己的脚手架。另在这一年的开发中，我们自己积累下来了一套RN组件库：[react-native-atlas](https://github.com/rcg1994/react-native-atlas)，也将直接嵌入到脚手架（[Atlas](https://github.com/rcg1994/atlas)）中。最终，将形成一套完整的RN项目流：组件库、项目脚手架、示例APP。其中示例APP将用于介绍组件库以及平时工作中积累下来的页面效果、demo、知识点等等。

由于自己的拖延症比较严重，也有点懒了。比如组件库仓库的最新更新时间是在一个月前了。为了让自己重新回到学习的状态，每天列计划是很有必要的一件事，希望自己不负初心，好好坚持下去。

#### 建立仓库并初始化

言归正传哈，代码托管在本人的 github 上：[https://github.com/rcg1994/atlas](https://github.com/rcg1994/atlas) 希望大家不要吝惜自己的 Star 哦！

以下是RN官方文档中的快速开始：[https://facebook.github.io/react-native/docs/getting-started.html](https://facebook.github.io/react-native/docs/getting-started.html)。官方推荐了两种方式，一种是使用 [Exop](https://expo.io/)，另一种是RN官方提供的命令行工具（react-native-cli）。我们选择第二种方式。

```javascript
npm install -g react-native-cli
react-native init atlas
```

静候安装完成。我们看下项目结构：

```
atlas
	|-- .babelrc
	|-- .buckconfig
	|-- .flowconfig
	|-- .watchmanconfig
	|-- android
	|-- App.js
	|-- app.json
	|-- index.js
	|-- ios
	|-- node_modules
	|-- package.json
	|-- yarn.lock
```

这是最简单的RN项目了，后续我们将慢慢添加功能进去，想想就很刺激。

直接输入`react-native run-android` 或 `react-native run-ios` 运行。

#### 命令行命令

为了未来开发的方便，我们将一些常用的命令行写到 `package.json` 中

```json
"scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start --port=8081",
    "debug": "open 'rndebugger://set-debugger-loc?host=localhost&port=8081'",
    "test": "jest",
    "clean": "rm -rf $TMPDIR/react-* && watchman watch-del-all && npm cache clean --force",
    "clean:android": "cd android/ && ./gradlew clean && cd .. && react-native run-android",
    "newclear": "rm -rf $TMPDIR/react-* && watchman watch-del-all && rm -rf ios/build && rm -rf node_modules/ && npm cache clean --force && npm i"
  }
```

后续将把项目的打包、页面快速生成等命令加入。

其中，调试工具 debug 使用的是：[react-native-debugger](https://github.com/jhen0409/react-native-debugger)，请预先在自己电脑上装好。

#### 待办

以下是Atlas的一些特点或打算引入的第三方库。

- [ ] 引入配套组件库 react-native-atlas
- [ ] 集成 Redux
- [ ] 集成 React-Navigation
- [ ] 集成 Redux-Saga
- [ ] 数据持久化
- [ ] 基于Plop的页面自动生成
- [ ] 错误奔溃采集
- [ ] 全局命令
- [ ] 引入 Iconfont 字体图标

#### 题外话

React Native (简称RN)是Facebook于2015年4月开源的跨平台移动应用开发框架，是Facebook早先开源的JS框架 React 在原生移动应用平台的衍生产物。作为风风火火了三四年的老兵，RN会过气么？

去年的新闻，各大公司纷纷弃用RN回归原生、RN将面临重大重构、Flutter的开源和发布...等等事件都让RN开发者心中难免有些顾忌。其实，我觉得吧，技术本就是不断更替的，大环境如此，个人的技术栈亦如此。而对于技术选型，往往要根据团队能力、项目特点、学习成本等因素综合考虑。比如创业公司如果需要快速开发新产品，为了节约成本，使用RN技术依然会是一个很好的选择。

面对技术更新，我们无需感到惊慌，要做的就是不断学习提升自己。