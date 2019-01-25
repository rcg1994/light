### 从零搭建RN脚手架-Atlas（四）：资源与字体图标

#### 图片资源

资源文件我们统一放在 `App/Resources` 下：

```javascript
|-- APP
	|-- Resources
		|-- Images // 图片存放
			|-- atlas@2x.png
			|-- atlas@3x.png
		|-- Images.js // 图片引用
		|-- index.js // 暴露接口
```

在存储图片时，您可使用类似IOS中@2x和@3x的方式命名不同像素比下的图片，RN会自动去选择正确的图片显示。当然，也可以正常引用。

`App/Resources/Images.js`

```javascript
const images = {
  atlas: require('./Images/atlas.png')
}

export default images

```

`App/Resources/index.js`

```javascript
import Images from './Images'

export { Images }

```

引用方式：比如主页上的logo

```javascript
import { Images } from '../Resources'
...
	<Image source={Images.atlas} style={Styles.logo} />
...
```

虽然上述方式可以比较有条理的管理图片资源了，但反复得引用，修改名称，还是会很乏味。比如说有一百张切图引进来，不断在 Images.js 中写引用关系，重复繁琐的工作太无趣了。我的想法是后面加上自动化，根据 Images 目录下的文件及文件名，自动生成 Images.js 文件。先记录在待办中，到时候和 Plop 一起写。

#### 字体图标

界面开发自然脱离不了图标啦，现在主要流行两种方式来管理图标。第一种就是直接使用图片，第二种便是使用字体图标。RN有一个字体图标的插件：[react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)。插件自带了很多流行的图标库，基本够日常使用的了。但对于设计稿还原度要求很高的项目，就需要自定义图标了。我们推荐使用阿里的 [Iconfont](https://www.iconfont.cn/)，已经有越来越多的设计师了解并使用起来了。（用不用，自己去diss设计师吧哈哈）

接下来说一下插件的使用以及如何自定义引入 Iconfont。

按照文档一步步走，很简单：

```javascript
yarn add react-native-vector-icons
react-native link react-native-vector-icons
```

我们在 `App/Pages/Home.js` 中使用：

```javascript
import Icon from 'react-native-vector-icons/FontAwesome'
...
	<Icon name="github" size={30} color="#333" />
...
```

重点来了：自定义 Iconfont。我们先在 Iconfont 上建一个项目，可以自己搜索别人分享出来的图标添加进去，也可以让设计师上传上去。然后选择下载至本地，可以获得一个带有demo的文件夹。解压之后如下：

```javascript
|-- demo_index.html
|-- iconfont.ttf
...
// 其他一些用不到的不列举了
```

#### 导入 iconfont 的字体图标

react-native-vector-icons 提供了方法让我们自定义字体：

`App/Resources/Iconfont.js`

```javascript
import createIconSet from 'react-native-vector-icons/lib/create-icon-set'
import glyphMap from './Iconfont/iconfont.json'

const IconfontSet = createIconSet(glyphMap, 'iconfont', 'iconfont.ttf')

export default IconfontSet

```

首先我们需要将字体文件引入到应用中。安卓直接将文件复制到 `android/app/src/main/assets/fonts` 下即可。IOS用xcode打开项目后，在 `Build Phases->Copy Bundle Resources` 下添加字体的引用就好了。我们将字体文件放了一份在 `App/Resources/Iconfont/iconfont.ttf`，ios即引用的这个文件，那么如果需要更新字体图标的话，就要覆盖以上两个位置的字体图标了。看看后面有没有办法解决。

字体引入之后，就需要建一个字体的映射map：

`App/Resources/Iconfont/iconfont.json`

```json
{
  "click": 58880
}
```

由于我们从阿里Iconfont上下载下来的字体的Unicode码是十六进制的，需要我们手动转化成十进制。当然，这里的转化我们也靠代码来自动完成。将我们之前下载的 `demo_index.html` 用浏览器打开，在控制台输入以下代码：

```javascript
let re = ''
let unicodes = document.querySelectorAll(".unicode>ul .dib .code-name")
let classes = document.querySelectorAll(".font-class>ul .dib .code-name")
unicodes.forEach((item,index)=>{
	let num = parseInt(item.innerHTML.replace('&amp;#x',''), 16)
	let name = classes[index].innerHTML.replace('.icon-','').replace("\n", "").replace(/ /g, "")
	re += '\n  "' + name +'": '+ num
	if(index+1<unicodes.length){re+=','}
})
console.log(re)
```

复制结果到 iconfont.json 里就好了。同样的，这还不够完美，等做到自动化的内容，我们将以上方式优化一下。

注：可能会因为Iconfont的网站更新导致上面的代码执行出错，重新自己分析下结构改改就好了。

使用：

```javascript
import {Iconfont} from '../Resources'
...   
	<Iconfont name="click" size={30} color="#333" />
...
```

至此，资源与字体图标都引用进来了。赶紧美化下APP，看下效果吧！

<Img src="https://github.com/rcg1994/light/blob/master/images/atlas.jpeg?raw=true" width="350"/>