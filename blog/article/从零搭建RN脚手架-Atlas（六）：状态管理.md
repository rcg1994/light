### 从零搭建RN脚手架-Atlas（六）：状态管理

#### redux+react-navigation

我们先把react-navigation 结合在一起，网上教程太多了，不赘述，直接上代码

修改 Navigation/AppNavigation.js，主要看后面的代码

```javascript
import React, { PureComponent } from 'react'
import { Text, Easing, Animated, BackHandler, Platform } from 'react-native'
import {
  createStackNavigator,
  createBottomTabNavigator
} from 'react-navigation'
import {
  createReduxContainer,
  createReactNavigationReduxMiddleware,
  createNavigationReducer
} from 'react-navigation-redux-helpers'
import { connect } from 'react-redux'
import { TabPages, AppPages } from './NavPages'
import Colors from '../Theme/Colors'
import { Iconfont } from '../Resources'

const TabNavigatorConfig = {
  defaultNavigationOptions: ({ navigation }) => ({
    tabBarIcon: ({ focused, tintColor }) => {
      const { routeName } = navigation.state
      const iconConf = {
        Home: 'atlas',
        Components: 'components',
        About: 'mine'
      }
      return (
        <Iconfont
          name={iconConf[routeName]}
          color={focused ? tintColor : '#777'}
          size={30}
        />
      )
    },
    tabBarLabel: ({ focused, tintColor }) => {
      const { routeName } = navigation.state
      const labelConf = {
        Home: '主页',
        Components: '路由',
        About: '状态'
      }
      return (
        <Text
          style={{
            color: focused ? tintColor : '#777',
            fontSize: 10,
            marginBottom: 2,
            textAlign: 'center',
            fontWeight: '200'
          }}
        >
          {labelConf[routeName]}
        </Text>
      )
    }
  }),
  tabBarOptions: {
    activeTintColor: Colors.primary,
    style: {
      backgroundColor: Colors.tabBackground,
      borderTopColor: Colors.borderLight
    }
  },
  animationEnabled: false,
  swipeEnabled: false,
  lazy: false
}

const AppNavigatorConfig = {
  headerMode: 'none',
  transitionConfig: () => ({
    transitionSpec: {
      duration: 300,
      easing: Easing.out(Easing.poly(4)),
      timing: Animated.timing
    },
    screenInterpolator: sceneProps => {
      const { layout, position, scene } = sceneProps
      const { index } = scene
      const width = layout.initWidth
      const translateX = position.interpolate({
        inputRange: [index - 1, index, index + 1],
        outputRange: [width, 0, -width]
      })
      const opacity = position.interpolate({
        inputRange: [index - 1, index - 0.99, index],
        outputRange: [0, 1, 1]
      })

      return { opacity, transform: [{ translateX }] }
    }
  })
}

const AppNavigator = createStackNavigator(
  {
    Root: {
      screen: createBottomTabNavigator(TabPages, TabNavigatorConfig)
    },
    ...AppPages
  },
  AppNavigatorConfig
)
// ========================Start==========================
export const routerReducer = createNavigationReducer(AppNavigator)

export const routerMiddleware = createReactNavigationReduxMiddleware(
  state => state.router,
  'root'
)

const App = createReduxContainer(AppNavigator, 'root')

// react-redux 7.0.0 中不推荐使用装饰器
// @connect(({ app, router }) => ({ app, router }))
class ReduxNavigation extends PureComponent {
  componentWillMount () {
    if (Platform.OS === 'ios') return
    BackHandler.addEventListener('hardwareBackPress', this.backHandle)
  }

  componentWillUnmount () {
    if (Platform.OS === 'ios') return
    BackHandler.removeEventListener('hardwareBackPress', this.backHandle)
  }

  backHandle = () => {
    const { dispatch, router } = this.props
    // change to whatever is your first screen, otherwise unpredictable results may occur
    if (router.routes.length === 1 && (router.routes[0].routeName === 'Root')) {
      return false
    }
    // if (shouldCloseApp(nav)) return false
    dispatch({ type: 'Navigation/BACK' })
    return true
  }

  render () {
    const { dispatch, router } = this.props
    return <App dispatch={dispatch} state={router} />
  }
}

const mapStateToProps = ({ router }) => ({
  router
})

export default connect(mapStateToProps)(ReduxNavigation)

// ========================End==========================

```

#### dva

我们不直接使用redux了，主要是redux的结构目录还是比较复杂，推荐使用dva框架。

参考 https://github.com/nihgwu/react-native-dva-starter

dva.js

```javascript

import React from 'react'
import { create } from 'dva-core'
import { Provider } from 'react-redux'

export default function (options) {
  const app = create(options)
  // HMR workaround
  if (!global.registered) options.models.forEach(model => app.model(model))
  global.registered = true

  app.start()
  // eslint-disable-next-line no-underscore-dangle
  const store = app._store

  app.start = container => () => <Provider store={store}>{container}</Provider>
  app.getStore = () => store

  return app
}

```

app.js

```javascript
import React from 'react'
import AppNavigation, { routerMiddleware, routerReducer } from './Navigation/AppNavigation'
import dva from './Store/dva'
import models from './Store'

const app = dva({
  initialState: {},
  models: models,
  extraReducers: { router: routerReducer },
  onAction: [routerMiddleware],
  onError (e) {
    console.log('onError', e)
  }
})

const App = app.start(<AppNavigation />)

export default App

```

其中 models 就是我们项目中要维护的数据状态。

我们将models定义在 /Store/Models 中，一个文件管理一类数据，具体请根据项目需求定义。

定义一个dva：appModel

```javascript
import { fetchFake } from '../../Services/appService'
import { ATModal } from 'react-native-atlas'

export default {
  namespace: 'app',
  state: {
    loading: true
  },
  reducers: {
    updateState (state, { payload }) {
      return { ...state, ...payload }
    }
  },
  effects: {
    * loadingAsync (action, { call, put }) {
      let loading = ATModal.loading({
        content: '异步加载中...',
        backdropPressToClose: false
      })
      let re = yield call(fetchFake)
      // Helper.
      if (re === 'success') {
        yield put({ type: 'updateState', payload: { loading: true } })
      }
      loading.close()
    }
  },
  subscriptions: {
    // setup ({ dispatch }) {
    //   dispatch({ type: 'loadStorage' })
    // }
  }
}

```



#### 使用

页面应用的时候，现将组件与redux链接（connect），通过props来获取数据和 dispatch action。

```javascript
import React from 'react'
import { View, Text } from 'react-native'
import { connect } from 'react-redux'
import { ATButton } from 'react-native-atlas'
import { Header } from '../Components'
import { AppStyles } from '../Theme'

class About extends React.Component {
  render () {
    return (
      <View style={AppStyles.screen}>
        <Header title="Redux & Dva" headerLeft={null} />
        <View style={AppStyles.body}>
          <View
            style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}
          >
            // *****************获取app模块中的loading数据*******************
            <Text>loading: {String(this.props.app.loading)}</Text>
						// ************************************
            <ATButton
              style={{ marginTop: 20 }}
              onPress={() => {
                // *****************dispatch*******************
                this.props.dispatch({
                  type: 'app/updateState',
                  payload: { loading: false }
                })
                // ************************************
              }}
            >action to false</ATButton>
            <ATButton
              style={{ marginTop: 20 }}
              onPress={() => {
                this.props.dispatch({
                  type: 'app/updateState',
                  payload: { loading: true }
                })
              }}
            >action to true</ATButton>
          </View>
        </View>
      </View>
    )
  }
}

export default connect(({ app }) => ({ app }))(About)

```

