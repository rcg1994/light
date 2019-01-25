#### Quokka

先shift+cmd+p （ctrl+shift+p）输入 quokka 选择 new javascript 就行了

#### ES7 React/Redux/GraphQL/React-Native snippets

用来快速编写。比如：

imp ->     import moduleName from 'module'

更多：[https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

#### Go Live

安装后 右下角有 Go Live 按钮，可以开启一个支持热更新的服务。

#### VSCode中"experimentalDecorators"语法检测报错

解决方法有两个：

1、在tsconfig.json中设置experimentalDecorators为true。

2、在VSCode里搜索experimentalDecorators, settings.json中设置javascript.implicitProjectConfig.experimentalDecorators为true。

#### eslint和格式化

直接按官方文档安装到项目就可以了

```
"eslint.autoFixOnSave": true,
"prettier.eslintIntegration": true,
```

设置这两个，一个是保存文件时自动修复。一个是格式化代码时按eslint规则来格式化。