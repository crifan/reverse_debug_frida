# frida-trace

* `frida-trace`
  * 官网文档
    * [frida-trace](https://frida.re/docs/frida-trace/)
  * 核心参数解释
    * 通用逻辑
      * `-m`、`-M`等参数，支持通配符
        * `*`：表示`所有的`（类或方法名）
    * 参数
      * `-m`
        * 语法：`-m OBJC_METHOD, --include-objc-method OBJC_METHOD`
          * include OBJC_METHOD
        * 含义：（要去hook的）包含的ObjC的（类和）方法
      * `-M`
        * 语法：`-M OBJC_METHOD, --exclude-objc-method OBJC_METHOD`
          * exclude OBJC_METHOD
        * 含义：（ 不要hook的）排除掉的ObjC的（类和）方法
      * `-O`
        * 语法：`-O FILE, --options-file FILE`
          * text file containing additional command line options
        * 含义：用Option文件，包含额外的命令行参数
          * 典型用法是，觉得`-m`、`-M`等要去hook和要去排除掉的类和方法，太多了，就转去放到Option文件中                        
  * 举例
    * 概述
      * hook调试`Preferences`：`-m`、`-M`等参数直接放到命令行中
        ```bash
        frida-trace -U -F com.apple.Preferences -m "*[AA* *]" -M "-[ASDBundle copyWithZone:]" -M "-[* copyWithZone:]" -M "-[AAUILabel *]"
        ```
      * hook调试`akd`：觉得`-m`、`-M`等参数太多，则放到`-O`的Option文件中
        ```bash
        frida-trace -U -n akd -O akdObjcMethods.txt
        ```
        * Option文件`akdObjcMethods.txt`
          ```txt
          -m "*[AA* *]"
          -m "*[AK* *]"
          -m "*[AS* *]"
          -m "*[NSXPC* *]"
          -M "-[NSXPC*coder *]"
          -M "-[NSXPCConnection replacementObjectForEncoder:object:]"
          ```
    * 详见
      * [frida-trace的iOS的ObjC的举例](../../../frida_example/frida_trace/ios_objc/README.md)

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
