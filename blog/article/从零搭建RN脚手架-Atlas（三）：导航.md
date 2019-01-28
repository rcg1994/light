### 从零搭建RN脚手架-Atlas（三）：导航

#### 引入 React-Navigation

从开始接触RN，就是用 [react-navigation](https://reactnavigation.org/) 做导航，基本满足了我们平时的业务需求。而且我开发过的APP导航这一块来说都相对简单，普遍是Tab+Stack的形式（类似微信）。脚手架中，我还是继续选择使用 react-navigation ，其他的导航库，可能后期会花点时间了解下。

按照官方文档：

```javascript
yarn add react-navigation
yarn add react-native-gesture-handler // 这个手势库之前没见过啊
react-native link react-native-gesture-handler
```

突然发现，现在 react-navigation 已经更新到 3.x 了，而之前项目用的版本还是 1.0 的～惭愧！需要找个时间再过一下3.x的文档才行。

#### 编写一个简单的包含导航的APP

我们的目标是完成一个拥有底部导航条的应用。为了方便，我首先直接引入了我们配套的组件库：[react-native-atlas](https://github.com/rcg1994/react-native-atlas)。

与导航有关的文件结构：

```javascript
|-- app.js // 入口文件
|-- App
    |-- Navigation
        |-- AppNavigation.js // 导航
		|-- NavPages.js // 页面定义
	|-- Pages
		|-- Library // 组件子页目录，便于定位与管理
			|-- Buttons.js // 介绍按钮组件的页面
		|-- About.js // 关于页
		|-- Components.js // 组件库页
		|-- Home.js // 主页
```

入口文件： `App/app.js` 

```javascript
import AppNavigation from "./Navigation/AppNavigation";

export default AppNavigation;
```

导航文件：`App/Navigation/AppNavigation.js`

```javascript
import React from "react";
import { View, Text, Easing, Animated } from "react-native";
import {
  createStackNavigator,
  createBottomTabNavigator,
  createAppContainer
} from "react-navigation";
import { TabPages, AppPages } from './NavPages'
import Colors from "../Theme/Colors";

const TabNavigatorConfig = {
  // ...
};

const AppNavigatorConfig = {
  // ...
};

const AppNavigator = createStackNavigator({
  Root: {
    screen: createBottomTabNavigator(TabPages, TabNavigatorConfig)
  },
  ...AppPages
}, AppNavigatorConfig);

export default createAppContainer(AppNavigator);

```

上面的一些配置信息就不在此展示了，具体可查看源码。

页面文件： `App/Pages/Home.js` 

```javascript
import React from "react";
import { View, Text } from "react-native";

class Home extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
        <Text>主页</Text>
      </View>
    );
  }
}

export default Home;

```

 `App/Pages/Components.js` 

```javascript
import React from "react";
import { View, Text } from "react-native";
import { ATButton } from "react-native-atlas";
import { Header } from "../Components";
import { AppStyles } from "../Theme";

class Components extends React.Component {
  render() {
    return (
      <View style={AppStyles.screen}>
        <Header title="组件库" headerLeft={null} />
        <View style={AppStyles.body}>
          <View
            style={{ flex: 1, alignItems: "center", justifyContent: "center" }}
          >
            <ATButton
              title="组件：按钮"
              onPress={() => {
                this.props.navigation.navigate("LibraryButtons");
              }}
            />
          </View>
        </View>
      </View>
    );
  }
}

export default Components;

```

`App/Pages/Library/Buttons.js`

```javascript
import React from "react";
import { View, Text } from "react-native";
import { Header } from "../../Components";
import AppStyles from "../../Theme/AppStyles";

class LibraryButtons extends React.Component {
  render() {
    return (
      <View style={AppStyles.screen}>
        <Header title="按钮" />
        <View style={AppStyles.body}>
          <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
            <Text>按钮</Text>
          </View>
        </View>
      </View>
    );
  }
}

export default LibraryButtons;

```

其他的页面文件都是类似的，存放在 `App/Pages` 下。

至此，我们完成了一个简单的带有导航功能的APP。如何将 Navigation 的信息存入 Redux 中，将在后面引入 Redux 时一起说明。

