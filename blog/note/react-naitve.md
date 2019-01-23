#### RN-IOS-Xcode升级之后报错

##### third-party/glog-0.3.4 C compiler cannot create executables

<https://github.com/facebook/react-native/issues/19774>



cd ./node_modules/react-native/third-party/glog-0.3.4 && ../../scripts/ios-configure-glog.sh



##### RCTWebsocket lib fishhook.a not found

goto Libraries > RCTWebSocket.xcodeproj > Targets > RCTWebSocket >Build Phases > link Binary with libraries

delete libfishhook.a , add libfishhook.a



#### xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun

运行 xcode-select --install 安装相关软件



#### xcrun: error: unable to find utility "instruments", not a developer tool or in PATH

[https://stackoverflow.com/questions/39778607/error-running-react-native-app-from-terminal-ios](https://stackoverflow.com/questions/39778607/error-running-react-native-app-from-terminal-ios)

```
sudo xcode-select -s /Applications/Xcode.app
xcode-select --install
sudo xcodebuild -license accept 
```



#### Xcode 10: third-party: 'config.h' file not found 

[https://github.com/facebook/react-native/issues/14382#issuecomment-313163119](https://github.com/facebook/react-native/issues/14382#issuecomment-313163119)

- In the Terminal, navigate to the `react-native/third-party/glog` folder inside `node_modules` (for me, this was `cd node_modules/react-native/third-party/glog-0.3.4`)
- Once actively in this folder, run `../../scripts/ios-configure-glog.sh`
- Glog is configured and the required `config.h` header file is created for Xcode to find

#### Iconfont转JSON

下载之后，打开html文件，控制台输入

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

