#### RN-IOS-Xcode升级之后报错

##### third-party/glog-0.3.4 C compiler cannot create executables

<https://github.com/facebook/react-native/issues/19774>



cd ./node_modules/react-native/third-party/glog-0.3.4 && ../../scripts/ios-configure-glog.sh



##### RCTWebsocket lib fishhook.a not found

goto Libraries > RCTWebSocket.xcodeproj > Targets > RCTWebSocket >Build Phases > link Binary with libraries

delete libfishhook.a , add libfishhook.a