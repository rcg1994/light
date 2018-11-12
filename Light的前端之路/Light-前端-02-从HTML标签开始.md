### 开始这一切吧！

没错，你没看错，我将从HTML标签开始我的整个系列文章。很基础吧？但是每个前端人都是从最简单的HTML标签开始的，都是从一个`<html></html>`开始整个前端宇宙，不是么？

文章最终会写成怎样呢？我也不知道哈哈哈，拭目以待吧！也当给自己找找状态了。（非初学者可以跳过）

### HTML长这样

***HTML***，全称`Hypertext Markup Language`，也就是“超文本链接标示语言”……这其实没啥好说的。。

简单来说，你访问一个网页，不管这个网页多酷炫，功能多复杂，它们都是从一个个HTML标签开始建立的。就像谷歌的真面目是这样的：

![谷歌](http://pi33bbo0e.bkt.clouddn.com/WX20181112-220832@2x.png)

页面就是由右边这些密密麻麻的标签组成的。浏览器再根据标签和样式规则渲染出对应的页面展示到我们眼前。

### HTML版本发展

从HTML最初的草案发布到现在较为成熟的HTML5，经历了整整25年（跟我差不多同龄了）。HTML4.0以及4.01其实已经算得上是比较友好的标准了，我们现在用到的一些基础标签，都是它的产物。比如：`h1～h6、p、a、table、div、span、img......`等等使用频率非常高的标签都是早早就实现了的。但是随着现代Web技术的更新和发展，更加复杂的网页需求以及更多的展示形式的出现，HTML已经很难承载这一切了。因此HTML5应运而生，作为最新的HTML标准，HTML5添加了新的语义、图形以及多媒体元素，也为旧有的标签添加了很多新的属性。

比如新的标签`canvas`解决了图形甚至动画的表达缺陷，新的API`localStorage`解决了web应用的本地存储问题，同时HTML5新加了很多符合语义化的标签`article、header、footer、section.....`。我们现在工作中，几乎所有项目也都是基于HTML5的，除非你要兼容一些版本很低很低的浏览器。

碰到兼容IE的需求，真的头疼。God bless you！

### HTML标签的分类

根据HTML各标签的布局特性，可以对它们进行分类。

##### 块级元素

块级元素大多是结构性标签，特征是能够识别和设置宽高，并且可以自动换行。块级元素可以包含行内元素和块级元素。

如：address、caption、div、h1～h6、dd,dl,dt、fieldset、form、legend、li、ol、ul、noframes、noscript、p、pre、table、tbody、tgoot、td、th、thead、tr

##### 行内元素

行内元素的特征是不能够识别和设置宽高，并且不会自动换行。行内元素可以包含行内元素，但不能包涵块级元素（真要写也不会出错，但是不符合标准）。

如：a、abbr、acronym、b、bdo、big、br、cite、code、dfn、em、i、kbd、label、q、samp、select、strong、sub、sup、textarea、tt

##### 行内块级元素

和行内元素差不多，但是它可以设置宽高。

如：img、input

一般还可以通过css修改元素样式`display:inline-block`实现

尽管标签区分了其是块级还是行内，但你仍可以通过设置它们的样式改变它们的展示形式。

### HTML语义化

说实话，我在工作中，还是比较忽视语义化的，基本都是DIV+CSS的方式去做开发。这里要检讨一下自己，存在即合理，有那么多语义化的标签存在，还是多去使用它们吧。选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。特别是做一些官网项目，或者SEO要求比较高的项目，尽量逼着自己去实现语义化。

参考：[如何理解 Web 语义化？](https://www.zhihu.com/question/20455165)

今天的没啥营养，貌似写这些出来也没多大意义，明天写写盒模型好了。

### 关于我

微信号：**rcgrcg**，欢迎交友～

![微信号 rcgrcg](http://upload-images.jianshu.io/upload_images/2180775-5b1b27daf44d6b93.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

为了生计，我也接外包项目的哦～

**网站搭建（PC、H5、前后端全包，我们有团队的哦），APP开发（安卓和IOS），都是有成功案例的哦。**

有兴趣的请联系我！！服务包您满意的那种！！

<div style="text-align: right;margin-top: 20px">Good luck！<br /> 2018-11-12 厦门</div>