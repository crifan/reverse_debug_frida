# 常见报错

## Process terminated=进程结束=崩溃退出

* 现象：`frida`或`frida-trace`调试时，时不时的遇到`Process terminated`，而停止退出调试
  * 现象一：`frida-trace -U -F com.apple.Preferences -m "*[AA* *]" -m "*[AK* *]" -m "*[AS* *]" -m "*[NS* *]" -M "-[ASDBundle copyWithZone:]" -M "-[ASDInstallationEvent copyWithZone:]"`导致崩溃
    * ![frida_trace_add_ns](../../assets/img/frida_trace_add_ns.png)
* 原因：暂不完全清楚
  * 可能是：
    * 要么是：`frida-trace`去hook的函数太多了，比如，加了`-m "*[NS* *]"`后导致崩溃
      * 注：iOS的ObjC的内部的多数，甚至是大多数，都是NS开头的，导致匹配到太多的类和函数，系统处理不过来了，导致frida崩溃。导致被调试的app崩溃。
        * 注：`NS`=`NextStep`，是iOS系统前身的苹果收购的NextStep公司名字
      * 解决办法：减少hook的范围=缩小匹配范围，比如改为：`-m "*[NSXPC* *]"`，暂时只关注我们要调试的`NSXPCConnection`的相关内容，基本上可以：避免崩溃

## Failed to spawn: unable to find process with name 'Preferences'

* 报错：`Failed to spawn: unable to find process with name 'Preferences'`
  * ![frida_failed_spawn_process_name](../../assets/img/frida_failed_spawn_process_name.png)
* 原因：frida命令用的是
  ```bash
  frida -U -l ./hookAccountLogin.js -n Preferences
  ```
  * 其中`-n`是加二进制名称，此处`Preferences`是app，所以属于参数使用错误，调试目标语法搞错了
* 解决办法：
  * 搞懂frida的调试目标方式，改为别的方式即可
    * 方式1：用`-N app_package_id`
      ```bash
      frida -U -l ./hookAccountLogin.js -N com.apple.Preferences
      ```
    * 方式2：换`-p PID`
      ```bash
      frida -U -l ./hookAccountLogin.js -p 18031
      ```
      * 其中是用iPhone中ssh中通过ps查看到
        ```bash
        ~ ps -A | grep Preferences
        ...
        18031 ??         0:02.43 /Applications/Preferences.app/Preferences
        ```
        * 得知`Preferences`的`PID`是`18031`
    * 方式3：用`-F`
      ```bash
      frida -U -l ./hookAccountLogin.js -F
      ```
      * 注：确保`Preferences`=系统的设置app，处于最前台在运行才能用`-F`


## ValueError: file descriptor cannot be a negative integer (-42)

* 问题：`frida-ps`、`frida-ls-devices`等frida-tools工具运行时报错：`ValueError: file descriptor cannot be a negative integer (-42)`
* 原因：当前`12.0.3`的`frida-tools`有bug
* 解决办法：升级到最新版`frida-tools`
* 具体步骤：
  ```bash
  pip install --upgrade frida_tools
  ```
  * 注：查看当前frida-tools的版本：
    ```bash
    pip show frida_tools
    ```

## Error: unable to intercept function at 0x202be0460; please file a bug

* 现象：Frida的hook的js代码中，函数地址写的是：
  ```js
  var akdSymbol2575_functionAddress = 0x1000a0460;
  ```
  * 导致报错：
    ```bash
    moduleName= akd moduleBaseAddress= 0x102b40000
    functionRealAddress)= 0x202be0460
    Error: unable to intercept function at 0x202be0460; please file a bug
        at value (frida/runtime/core.js:367)
    ```
* 原因：函数地址写错了 -》 找不到函数地址 -》 所以报错
* 解决办法：确保函数地址是正确的
* 具体做法：代码改为：
  ```js
  var akdSymbol2575_functionAddress = 0xa0460;
  ```
  * 说明
    * `0x1000a0460` 是 加了`VM` 虚拟地址后的 akd二进制内函数的地址
      * `0xa0460`：是不带VM的，真正的函数内的偏移地址

