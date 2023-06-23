# Frida的典型使用方式

在搞懂了[调试目标方式](../../use_frida/frida_cli/debug_target.md)和[写js脚本](../../use_frida/frida_cli/js_script.md)后，再来介绍

* Frida的典型使用方式
  * 用哪个命令？
    * 主要是用：`frida`
      * 大都是都是`frida`然后加上`-l xxx.js`加上js脚本，实现自己特定的调试的目的
    * 偶尔使用：`frida-trace`
      * 偶尔直接用`frida-trace`，加上要hook的类或函数，去追踪代码执行过程中，调用到哪些函数
  * 常见写法 = 怎么用？
    * 命令行中使用
      * `frida xxx`
      * `frida-trace xxx`
  * （其中`xxx`中）常用参数有
    * 要调试的设备用：`-U`
      * `-U`：调试的是USB连接的移动端的设备（比如iPhone）
    * 要调试的目标app/二进制：`-p PID` / `-n executableName` / `-N com.app.identifier` / `-F` / `-F com.app.identifier`
      * 有多种写法，选择合适的一个即可
        * app或二进制均支持
          * 通过PID指定进程：`-p PID`
        * 仅支持二进制
          * 通过`-n`指定二进制可执行文件名：`-n executableName`
        * 仅支持app
          * 通过`-N`指定app的包名：`-N com.app.identifier`
          * 通过`-F`指定当前最顶层正在运行的app：`-F`
            * 有时候可以额外给`-F`再加上app的包名，效果更好：`-F com.app.identifier`
    * -》所以常见写法举例
      * `frida`
        * 无需js：直接动态交互式的调试
          ```bash
          frida -U -f com.apple.Preferences
          ```
        * 需要js：实现特定调试目的
          * 自己写js
            * 使用`-p`指定进程PID
              ```bash
              frida -U -F -p 18533 -l ./hookAccountLogin_Interceptor.js
              ```
                * `-U`：调试的设备是（当前Mac用）USB连接的iPhone
                * `-F -p 18533`：调试的app是
                  * `-F`：表示是当前最前端显示的app
                  * `-p 18533`：（额外）加上PID，指定app的进程ID
                * `-l ./hookAccountLogin_Interceptor.js`：所要具体调试的内容，放在js中
            * 使用`-n`指定二进制文件名=进程名=ProcessName
              ```bash
              frida -U -n akd -l ./fridaStalker_akdSymbol2575.js
              ```
                * `-n akd`：要调试的目标是，进程名=二进制名是`akd`的进程
                  * 注：`akd`=`AuthKit`的daemon进程
          * 借助别人的js
            ```bash
            frida --codeshare lichao890427/ios-utils -F com.apple.Preferences
            ```
      * `frida-trace`
        * 用frida-trace追踪Preferences设置中Apple账号登录过程调用了哪些函数
          ```bash
          frida-trace -U -F com.apple.Preferences -m "*[AA* *]" -m "*[AK* *]" -m "*[AS* *]" -m "*[NSXPC* *]" -M "-[ASDBundle copyWithZone:]" -M "-[ASDInstallationEvent copyWithZone:]" -M "-[NSXPCEncoder _encodeArrayOfObjects:forKey:]" -M "-[NSXPCEncoder _encodeUnkeyedObject:]" -M "-[NSXPCEncoder _replaceObject:]" -M "-[NSXPCEncoder _checkObject:]" -M "-[NSXPCEncoder _encodeObject:]" -M "-[NSXPCConnection replacementObjectForEncoder:object:]"
          ```
          * 说明
            * `-U -F com.apple.Preferences`：调试目标设备和目标app是和frida写法一致
            * 但是后续要trace的函数写法，是frida-trace所特有的语法：
              * `-m xxx`：include=包含要调试的Objc的类
              * `-M xxx`：exclue=排除掉Objc的类，不调试
