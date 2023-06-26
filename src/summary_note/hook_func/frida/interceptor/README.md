# Interceptor

## 心得

### 通过函数地址去hook拦截函数

* 已知：函数的（二进制内偏移量）地址
* 想要：去hook拦截函数
* 举例说明
  * akd中函数`___lldb_unnamed_symbol2575$$akd`的二进制内偏移量是：`0xa0460`
    * 思路：动态计算加上模块后的真实函数地址，再去hook
    * 核心代码：
      ```js
      console.log("dynamicDebug/frida/scripts/fridaStalker_akdSymbol2575.js");
      // var akdSymbol2575_functionAddress = 0x1000a0460;
      var akdSymbol2575_functionAddress = 0xa0460;
      // arm64 akd: ___lldb_unnamed_symbol2575$$akd
      const moduleName = "akd";
      const moduleBaseAddress = Module.findBaseAddress(moduleName);
      console.log("moduleName=", moduleName, "moduleBaseAddress=", moduleBaseAddress);
      const functionRealAddress = moduleBaseAddress.add(akdSymbol2575_functionAddress);
      console.log("functionRealAddress=", functionRealAddress);
      Interceptor.attach(functionRealAddress, {
          onEnter: function(args) {
              var arg0 = args[0]
              var arg1 = args[1]
              var arg2 = args[2]
              console.log("arg0=" + arg0 + ", arg1=" + arg1 + ", arg2=" + arg2);
              var curTid = Process.getCurrentThreadId();
              console.log("curTid=", curTid);
      ...
      ```
      * 输出
        ```bash
        dynamicDebug/frida/scripts/fridaStalker_akdSymbol2575.js
        moduleName= akd moduleBaseAddress= 0x102b40000
        functionRealAddress= 0x102be0460
        arg0=0xfffffffffffffffe, arg1=0x16d346838, arg2=0x16d346838
        curTid= 35847
        ...
        ```
    * 完整代码和输出，详见：[___lldb_unnamed_symbol2575$$akd](../../../../frida_example/frida/ios_objc/stalker/akd_lldb_unnamed_symbol2575.md)

### onLeave中如何获取到self类本身的实例变量

* 思路：`onEnter`时，把self变量赋值保存给`this.xxx`，`onLeave`时去读取即可
* 代码
  ```js
  Interceptor.attach(curMethod.implementation, {
    onEnter: function(args) {
        this.argSelfObj = argSelfObj
        ...
    },

    onLeave: function (retval) {
      var retValObj = ObjC.Object(retval);
      var retValObjStr = retValObj.toString();
      console.log("onLeave: retValObj=" + retValObj + ",retValObjStr=" + retValObjStr)
      const curSelfObj = this.argSelfObj
      console.log("curSelfObj=" + curSelfObj)
    }
  })
  ```
* 输出
  ```bash
  ==================== -[NSXPCConnection setExportedObject:] ====================
  argSelfObj:  <ACTrackedXPCConnection: 0x154907c50> connection to service named com.apple.accountsd.accountmanager
  onLeave: retValObj=nil,retValObjStr=nil
  curSelfObj=<ACTrackedXPCConnection: 0x154907c50> connection to service named com.apple.accountsd.accountmanager
  ```

## 问题

### TypeError: cannot read property 'implementation' of undefined

* 问题：
  
写frida的hook代码调试ObjC的函数

```js
function hook_specific_method_of_class(className, funcName)
{
    var hook = ObjC.classes[className][funcName];
    Interceptor.attach(hook.implementation, {
      onEnter: function(args) {
        console.log("[*] Detected call to: " + className + " -> " + funcName);
      }
    });
}

//Your class name  and function name here
hook_specific_method_of_class("AAUISignInViewController", "_nextButtonSelected:")
```

始终报错：

`TypeError: cannot read property 'implementation' of undefined`

* 原因：此处iOS的ObjC的函数名`_nextButtonSelected:`写法有误
* 解决办法：改为正确的写法：`- _nextButtonSelected:`
  * 注：此处的ObjC的类的函数名的写法：
    * 加上类型是`class`的还是`instance`，所对应的`加号`=`+` 还是 `减号`=`-`，以及中间带上`空格`=` `
  * 相关代码改动：
    ```js
    hook_specific_method_of_class("AAUISignInViewController", "- _nextButtonSelected:")
    ```
  * 完整代码详见：[单个类的单个函数](../../../../frida_example/frida/ios_objc/hook_func/single_cls_single_func.md)

### frida去hook函数时确保已执行函数但不触发不输出日志log

* 问题：之前用了`ApiResolver`去写了hook代码，后续调试的函数确保被触发了，但是却没输出我们希望的日志
* 原因：此处`ApiResolver`只是search查找，函数是否存在，只运行一次，后续的确，本身就不会再次触发，所以后续代码被触发时，就不会打印日志
* 解决办法：真正的要实现，hook函数，待函数被运行后触发日志打印，应该改用：`Interceptor`
  * 注：
    * `ApiResolver`的完整代码和解释，详见：[ApiResolver](../../../../frida_example/frida/ios_objc/apiresolver/README.md)
    * `Interceptor`的完整示例代码，详见：[Interceptor=hook函数](../../../../frida_example/frida/ios_objc/hook_func/README.md)
