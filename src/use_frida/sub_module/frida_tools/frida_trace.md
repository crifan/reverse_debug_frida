# frida-trace

* `frida-trace`
  * 官网文档
    * [frida-trace](https://frida.re/docs/frida-trace/)

## 用法举例

## 调试`akd`进程

命令：

```bash
frida -U -n akd -l ./fridaStalker_akdSymbol2575.js
```

## 调试`Preferences`设置app

用frida-trace追踪Preferences设置中Apple账号登录过程调用了哪些函数

* 写法1：要包含和排除的ObjC的类，都直接加到命令行中
  ```bash
  frida-trace -U -F com.apple.Preferences -m "*[AA* *]" -m "*[AK* *]" -m "*[AS* *]" -m "*[NSXPC* *]" -M "-[ASDBundle copyWithZone:]" -M "-[ASDInstallationEvent copyWithZone:]" -M "-[NSXPCEncoder _encodeArrayOfObjects:forKey:]" -M "-[NSXPCEncoder _encodeUnkeyedObject:]" -M "-[NSXPCEncoder _replaceObject:]" -M "-[NSXPCEncoder _checkObject:]" -M "-[NSXPCEncoder _encodeObject:]" -M "-[NSXPCConnection replacementObjectForEncoder:object:]"
  ```
  * 说明
    * `-U -F com.apple.Preferences`：调试目标设备和目标app是和frida写法一致
      * `-U`：调试设备是（Mac中通过）USB连接的iPhone
      * `-F com.apple.Preferences`：调试的app是当前iPhone中，最前端所显示的app = 设置app = 包名是`com.apple.Preferences`
    * 但是后续要trace的函数写法，是frida-trace所特有的语法：
      * `-m xxx`：include=包含要调试的Objc的类
      * `-M xxx`：exclue=排除掉Objc的类，不调试
* 写法2：觉得参数写的太长= 包含和排除的ObjC的类太多，所以都放到一个单独的文件中
  ```bash
  frida-trace -U -F com.apple.Preferences -O Preferences_accoutLogin_hook.txt
  ```
    * `Preferences_accoutLogin_hook.txt`中的内容是
      ```txt
      -m "*[AA* *]"
      -m "*[AK* *]"
      -m "*[AMS* *]"
      -m "*[AS* *]"
      -m "*[AC* *]"
      -m "*[NSError *]"
      -M "-[* classForCoder]"
      -M "-[* classForKeyedArchiver]"
      -M "-[AAUISignInViewController numberOfSectionsInTableView:]"
      -M "-[AAUISignInViewController tableView*]"
      -M "-[NSPath* *]"
      -M "-[* copyWithZone:]"
      -M "-[*NSCFString *]"
      -M "-[*NSCFError *]"
      -M "-[* *ayoutSubviews]"
      -M "-[* intrinsicContentSize]"
      -M "-[* viewForLastBaselineLayout]"
      -M "-[* contentLayoutGuide]"
      -M "-[* _updateStyleIfNeeded]"
      -M "-[* _updateStackViewSpacing]"
      -M "-[* _updateLabelFonts]"
      -M "-[AAUILabel *]"
      -M "-[AAUIBuddyView *]"
      -M "-[AAUIHeaderView *]"
      -M "-[* .cxx_destruct]"
      -M "-[* dealloc]"
      -M "-[* supportsSecureCoding]"
      -M "-[* initWithCoder:]"
      -M "-[ACAccountStore _clearAccountCaches]"
      ```

## help语法帮助

```bash
➜  ~ frida-trace --help
usage: frida-trace [options] target

positional arguments:
  args                  extra arguments and/or target

options:
  -h, --help            show this help message and exit
  -D ID, --device ID    connect to device with the given ID
  -U, --usb             connect to USB device
  -R, --remote          connect to remote frida-server
  -H HOST, --host HOST  connect to remote frida-server on HOST
  --certificate CERTIFICATE
                        speak TLS with HOST, expecting CERTIFICATE
  --origin ORIGIN       connect to remote server with “Origin” header set to ORIGIN
  --token TOKEN         authenticate with HOST using TOKEN
  --keepalive-interval INTERVAL
                        set keepalive interval in seconds, or 0 to disable (defaults to -1 to auto-select based on transport)
  --p2p                 establish a peer-to-peer connection with target
  --stun-server ADDRESS
                        set STUN server ADDRESS to use with --p2p
  --relay address,username,password,turn-{udp,tcp,tls}
                        add relay to use with --p2p
  -f TARGET, --file TARGET
                        spawn FILE
  -F, --attach-frontmost
                        attach to frontmost application
  -n NAME, --attach-name NAME
                        attach to NAME
  -N IDENTIFIER, --attach-identifier IDENTIFIER
                        attach to IDENTIFIER
  -p PID, --attach-pid PID
                        attach to PID
  -W PATTERN, --await PATTERN
                        await spawn matching PATTERN
  --stdio {inherit,pipe}
                        stdio behavior when spawning (defaults to “inherit”)
  --aux option          set aux option when spawning, such as “uid=(int)42” (supported types are: string, bool, int)
  --realm {native,emulated}
                        realm to attach in
  --runtime {qjs,v8}    script runtime to use
  --debug               enable the Node.js compatible script debugger
  --squelch-crash       if enabled, will not dump crash report to console
  -O FILE, --options-file FILE
                        text file containing additional command line options
  --version             show program's version number and exit
  -I MODULE, --include-module MODULE
                        include MODULE
  -X MODULE, --exclude-module MODULE
                        exclude MODULE
  -i FUNCTION, --include FUNCTION
                        include [MODULE!]FUNCTION
  -x FUNCTION, --exclude FUNCTION
                        exclude [MODULE!]FUNCTION
  -a MODULE!OFFSET, --add MODULE!OFFSET
                        add MODULE!OFFSET
  -T INCLUDE_IMPORTS, --include-imports INCLUDE_IMPORTS
                        include program's imports
  -t MODULE, --include-module-imports MODULE
                        include MODULE imports
  -m OBJC_METHOD, --include-objc-method OBJC_METHOD
                        include OBJC_METHOD
  -M OBJC_METHOD, --exclude-objc-method OBJC_METHOD
                        exclude OBJC_METHOD
  -j JAVA_METHOD, --include-java-method JAVA_METHOD
                        include JAVA_METHOD
  -J JAVA_METHOD, --exclude-java-method JAVA_METHOD
                        exclude JAVA_METHOD
  -s DEBUG_SYMBOL, --include-debug-symbol DEBUG_SYMBOL
                        include DEBUG_SYMBOL
  -q, --quiet           do not format output messages
  -d, --decorate        add module name to generated onEnter log statement
  -S PATH, --init-session PATH
                        path to JavaScript file used to initialize the session
  -P PARAMETERS_JSON, --parameters PARAMETERS_JSON
                        parameters as JSON, exposed as a global named 'parameters'
  -o OUTPUT, --output OUTPUT
                        dump messages to file
```
