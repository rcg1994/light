### 从零搭建RN脚手架-Atlas（五）：RN版本升级

#### 自我检讨

阿西吧！上一次更新已经是五个月前了。。。人的惰性真的是很可怕的东西，当初还很有信心能坚持下去，然而现实总是残酷。。换工作之后，已经很久没用RN开发项目了。这次也是因为公司需求，才会重新把注意力转回RN上来。经过五个月的空窗，RN版本都更新了好几版，因此很有必要升级下脚手架的版本了。

#### 注意事项

版本升级还是要谨慎一点，特别是已经在线上跑的产品。以下是更新版本的几个要点：

* 原生应用中引用的RN版本要与项目中的RN版本一致
* 使用热更新的项目，升级后的RN包不能热更新到正在运行的项目中，因为线上的RN版本是老版本了，和最新的不兼容

#### 如何升级

1. 将要升级的版本添加到项目中：`yarn add react-native@0.59.4 react@16.8.3`。我这里是选择了ignite最新包含的rn版本。

2. 由于0.59版本需要 android 28 的支持，因此需要提升下安卓的编译版本号

   ```java
   buildscript {
       ext {
           buildToolsVersion = "28.0.3"
           minSdkVersion = 16
           compileSdkVersion = 28
           targetSdkVersion = 28
           supportLibVersion = "28.0.0"
       }
       dependencies {
           classpath 'com.android.tools.build:gradle:3.3.1'
       }
   }
   ```

3. 提升下 gradle 的版本号 (android/gradle/wrapper/gradle-wrapper.properties)

   ```java
   distributionUrl=https\://services.gradle.org/distributions/gradle-4.10.1-all.zip
   ```

4. 在升级过程中你可能会遇到别的乱七八糟的问题，耐心查找解决方案吧。经过上述步骤，我这边的安卓编译已经可以成功了，但是手机调试还是遇到了个bug。安卓机无法监听到 Metro 服务。

   解决办法：https://github.com/facebook/react-native/issues/23380#issuecomment-473871592

   在安卓 manifest 中添加 `android:usesCleartextTraffic="true"`

   查了下usesCleartextTraffic的作用：从Android 6.0开始引入了对Https的推荐支持，与以往不同，Android P的系统上面默认所有Http的请求都被阻止了。usesCleartextTraffic属性的默认值从true改变为false

5. 至此，貌似没问题了。。然鹅。。安卓提示`BackAndroid is deprecated and has been removed from this package,Use BackHandler instead`

   目测是很多第三方库的版本太低了，索性把第三方库都升级下。

   最终的问题出在我们自己的组件库上：删除 react-native-modalbox.js 文件内的 BackAndroid 引用

