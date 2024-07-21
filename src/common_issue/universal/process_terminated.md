# Process terminated

关于`Process terminated`=`进程结束`=`崩溃退出`，目前遇到多种现象和可能原因：

## 由于hook函数太多导致崩溃

* 现象一：用frida-trace去hook太多的类Obj的函数
  ```bash
  frida-trace -U -F com.apple.Preferences -m "*[AA* *]" -m "*[AK* *]" -m "*[AS* *]" -m "*[NS* *]" -M "-[ASDBundle copyWithZone:]" -M "-[ASDInstallationEvent copyWithZone:]"
  ```
  * 导致崩溃退出报错：`Process terminated`
    * ![frida_trace_add_ns](../../assets/img/frida_trace_add_ns.png)
* 原因：`frida-trace`去hook的函数太多了，估计是，加了`-m "*[NS* *]"`后导致崩溃
  * 注：iOS的ObjC的内部的多数，甚至是大多数，都是`NS`开头的，导致匹配到太多的类和函数，系统处理不过来了，导致frida崩溃，同时导致被调试的app崩溃。
    * 注：`NS`=`NextStep`，是iOS系统前身的苹果收购的NextStep公司名字
  * 解决办法：减少hook的范围=缩小匹配范围，比如此处改为：`-m "*[NSXPC* *]"`，暂时只关注我们要调试的`NSXPCConnection`的相关内容，基本上可以：避免崩溃

## frida的`new ObjC.Object`方面的bug

* 现象：
  * 概述`frida`调试时，由于加了`new ObjC.Object(someArg)`的`ptr`转换成`ObjC的对象`，结果就会时不时的遇到`Process terminated`，而崩溃停止退出调试
  * 详解=具体现象

代码：

```js
...
function hook_class_method(class_name, method_name)
{
    var hook = ObjC.classes[class_name][method_name];
    Interceptor.attach(hook.implementation, {
        onEnter: function(args) {
            console.log("=========== [*] Detected call to: " + class_name + " -> " + method_name);
            //objc的函数，第0个参数是id，第1个参数是SEL，真正的参数从args[2]开始
            const argId = args[0];
            // console.log("argId: ", argId);

            const argSel = args[1];
            // console.log("argSel: ", argSel);

            const argSelStr = ObjC.selectorAsString(argSel);
            console.log("argSelStr: ", argSelStr);


            const argCount = occurrences(argSelStr, ":");
            console.log("argCount: ", argCount);


            for (let curArgIdx = 0; curArgIdx < argCount; curArgIdx++) {
                const curArg = args[curArgIdx + 2];
                // console.log("[%d] curArg: ", curArgIdx, curArg);
                // console.log("[%d]=", curArgIdx, " ,curArg=", curArg);
                // console.log("[%d]=" + curArgIdx + " ,curArg=" + curArg);
                console.log("---------- [" + curArgIdx + "] curArg=" + curArg);
                if (curArg && (curArg != 0x0)) {
                    console.log("curArg className: ", curArg.$className);
                    const curArgObj = new ObjC.Object(curArg);
                    console.log("curArgObj: ", curArgObj);
                    console.log("curArgObj className: ", curArgObj.$className);
                }
            }
        }
    });
}
...
```

可以hook输出部分log日志，但是很快，时不时的，就崩溃退出了：

```bash
✘ crifan@licrifandeMacBook-Pro  ~/dev/dev_root/iosReverse/AppleStore/Preferences_app/dynamicDebug/frida  frida -U -l hookAccountLogin_NSURL.js -F
     ____
    / _  |   Frida 16.0.10 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to iPhone (id=abdc0dd961c3cb96f5c4afe109de4eb48b88433a)
[*] Started: Hook all methods of a specific class
[+] Class Name: NSURL
    [*] Omit hooking  + allocWithZone:
    [*] Omit hooking  - _cfTypeID
    [*] Omit hooking  - retain
    [*] Omit hooking  - release
    [*] Omit hooking  - copyWithZone:
[*] Completed: Hook all methods of a specific class
[iPhone::设置 ]-> =========== [*] Detected call to: NSURL -> - scheme
argSelStr:  scheme
argCount:  0
=========== [*] Detected call to: NSURL -> - _cfurl
argSelStr:  _cfurl
argCount:  0
=========== [*] Detected call to: NSURL -> - scheme
argSelStr:  scheme
argCount:  0
=========== [*] Detected call to: NSURL -> - _cfurl
argSelStr:  _cfurl
argCount:  0
=========== [*] Detected call to: NSURL -> + fileURLWithPath:isDirectory:
argSelStr:  fileURLWithPath:isDirectory:
argCount:  2
---------- [0] curArg=0x282336580
curArg className:  undefined
curArgObj:  /var/mobile/Library/Caches/com.apple.AppleAccount
curArgObj className:  NSPathStore2
---------- [1] curArg=0x0
=========== [*] Detected call to: NSURL -> - initFileURLWithPath:isDirectory:
argSelStr:  initFileURLWithPath:isDirectory:
argCount:  2
---------- [0] curArg=0x282336580
curArg className:  undefined
curArgObj:  /var/mobile/Library/Caches/com.apple.AppleAccount
curArgObj className:  NSPathStore2
---------- [1] curArg=0x0
=========== [*] Detected call to: NSURL -> - setResourceValues:error:
argSelStr:  setResourceValues:error:
argCount:  2
Process terminated
[iPhone::设置 ]->


Thank you for using Frida!
```

![frida_objc_process_terminated](../../assets/img/frida_objc_process_terminated.png)

* 原因：暂不完全清楚
  * 可能原因：`frida`的`ObjC`的`Objcect`转换方面的bug，暂时无法解决
    * 详见：
      * 【未解决】frida中hook函数打印参数值时最后app崩溃frida输出Process terminated
      * 【未解决】frida中hook调试iOS的ObjC的函数参数时始终出现崩溃Process terminated
