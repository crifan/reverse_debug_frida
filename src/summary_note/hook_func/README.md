# hook函数

## Interceptor.attach

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
