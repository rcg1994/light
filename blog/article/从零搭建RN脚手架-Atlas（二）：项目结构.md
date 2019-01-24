### 从零搭建RN脚手架-Atlas（二）：项目结构

#### 基础结构

在一个APP中，基本都是以页面为单元来划分的，因此需要一个目录专门存放页面（Pages）。在不同的页面中大概率会使用公共的组件，因此需要一个组件目录（Components）。还有与服务端之间的通信（Services）、应用数据的管理（Store）、配置信息（Config）等等。先简写一个项目结构：

```javascript
|--App
	|-- Components // 组件库
	|-- Config // 项目配置
	|-- Pages // 页面
	|-- Services // 服务与接口
	|-- Store // 数据管理
	|-- Theme // 样式主题
	|-- Resources // 图片等资源
```

#### 给项目贴金

以上的项目结构基本满足了简单应用的开发需求，我将尝试着添加一些第三方库进去，或更多有意思的功能。对于数据的统一管理，将采用 [Redux](https://github.com/reduxjs/redux) 去实现，并用 [Redux-Saga](https://github.com/redux-saga/redux-saga) 去实现异步操作。导航方面，我们选择 [React-Navigation](https://github.com/react-navigation/react-navigation)。这些其实都算基本功能了。

当这类第三方库引入项目之后，其实后面开发做的事情，更多的就是“依葫芦画瓢”。那难免有一些重复的操作，或者说是类似复制粘贴这样的工作。相信每个开发者都遇到过。那么就需要一些自动化的工具来帮助我们去做这些重复的工作。比如新建一个页面，页面对应的数据、导航信息、模版结构等等都可以自动生成。[Plop](https://github.com/amwmedia/plop) 将是一个很棒的选择，具体如何实现我还没去研究，但大致方向是这样的：根据日常开发经验，编写一些通用模版，利用Plop去生成新的组件、页面、数据等等。

再来谈一下我们配套的组件库：[react-native-atlas](https://github.com/rcg1994/react-native-atlas)，它的一个特点就是组件主题的可配置性。开发人员可通过配置文件去实现主题的修改，例如按钮的默认高度、默认颜色，或者是可跳转列表的箭头大小等等。因此，项目中将会有一个目录去统一管理这个组件库的配置。

#### 关于组件库

[react-native-atlas](https://github.com/rcg1994/react-native-atlas) 是UI层面的组件库，并且没有使用到原生方法。因此，应用时只要安装引入即可，无需link。

#### 完整的项目结构

```javascript
|-- AtlasConfig // react-native-atlas 的配置目录
|-- Generators // plop 的模板文件
|-- Script // 一些命令行工具
|-- App // 应用目录
	|-- Components // 组件库
	|-- Config // 项目配置
	|-- Pages // 页面
	|-- Navigation // 导航文件
	|-- Services // 服务与接口
	|-- Store // 数据管理
		|-- Redux // 存放 redux
		|-- Sagas // 存放 redux 异步操作
	|-- Theme // 样式主题
	|-- Resources // 图片等资源
	|-- Lib // 一些工具类
```

什么安卓文件夹、ios文件夹就不写啦。

后面我会详细写下各种第三方库的引入，敬请期待。