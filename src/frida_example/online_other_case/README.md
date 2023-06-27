# 网上别人的案例

* 网上的别人的Frida的使用案例
  * 官网、官网
    * （Frida作者）oleavr写的，实例代码
      * [oleavr/frida-agent-example: Example Frida agent written in TypeScript](https://github.com/oleavr/frida-agent-example)
    * [frida-presentations](https://github.com/frida/frida-presentations)
  * [Frida在iOS平台进行OC函数hook的常用方法 | 8Biiit's Blog](https://8biiit.github.io/2019/08/12/Frida/)
  * [personal_script/Frida_script at master · lich4/personal_script · GitHub](https://github.com/lich4/personal_script/tree/master/Frida_script)
    * ios_utils
      * https://github.com/lich4/personal_script/blob/master/Frida_script/ios_utils.js
    * antijailbreak
      * https://github.com/lich4/personal_script/blob/master/Frida_script/antijailbreak.js
    * getdeviceinfo
      * https://github.com/lich4/personal_script/blob/master/Frida_script/getdeviceinfo.js
    * ios_dump
      * https://github.com/lich4/personal_script/blob/master/Frida_script/ios_dump.js
    * recvfrom
      * https://github.com/lich4/personal_script/blob/master/Frida_script/recvfrom.js
    * sendto
      * https://github.com/lich4/personal_script/blob/master/Frida_script/sendto.js
    * android_hook_detect
      * https://github.com/lich4/personal_script/blob/master/Frida_script/android_hook_detect.js
  * [frida-ios-hook/frida-scripts](https://github.com/noobpk/frida-ios-hook/tree/master/frida-ios-hook/frida-scripts)
    * ssl pinning
      * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/bypass-ssl-ios13.js
    * class + method
      * find
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/find-all-classes-methods.js
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/find-all-classes.js
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/find-app-classes-methods.js
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/find-app-classes.js
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/find-specific-method.js
      * hook
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/hook-all-methods-of-all-classes-app-only.js
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/hook-all-methods-of-specific-class.js
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/hook-specific-method-of-class.js
      * dump=show
        * https://github.com/noobpk/frida-ios-hook/blob/master/frida-ios-hook/frida-scripts/show-all-methods-of-specific-class.js
  * [0xdea/frida-scripts](https://github.com/0xdea/frida-scripts)
    * https://github.com/0xdea/frida-scripts/blob/master/raptor_frida_android_enum.js
    * https://github.com/0xdea/frida-scripts/blob/master/raptor_frida_android_trace.js
  * 用frida实现各种功能
    * [misc/frida-read-process-memory.py at master · poxyran/misc · GitHub](https://github.com/poxyran/misc/blob/master/frida-read-process-memory.py)
      * counts the number of basic blocks
        * [misc/count_func_bb.py](https://github.com/poxyran/misc/blob/master/count_func_bb.py)
      * enumerate-imports
        * [misc/frida-enumerate-imports.py](https://github.com/poxyran/misc/blob/master/frida-enumerate-imports.py)
      * enumerate-modules
        * [misc/frida-enumerate-modules.py](https://github.com/poxyran/misc/blob/master/frida-enumerate-modules.py)
      * heap-trace
        * [misc/frida-heap-trace.py](https://github.com/poxyran/misc/blob/master/frida-heap-trace.py)
      * memory-scan
        * [misc/frida-memory-scan.py](https://github.com/poxyran/misc/blob/master/frida-memory-scan.py)
      * read-process-memory
        * [misc/frida-read-process-memory.py](https://github.com/poxyran/misc/blob/master/frida-read-process-memory.py)
      * write-process-memory
        * [misc/frida-write-process-memory.py](https://github.com/poxyran/misc/blob/master/frida-write-process-memory.py)
      * frida-stalker-example
        * [misc/frida-stalker-example.py](https://github.com/poxyran/misc/blob/master/frida-stalker-example.py)
  * [frida-snippets/README.md at master · iddoeldor/frida-snippets · GitHub](https://github.com/iddoeldor/frida-snippets/blob/master/README.md)
    * Native
      * Load C/C++ module
      * One time watchpoint
      * Socket activity
      * Intercept open
      * Execute shell command
      * List modules
      * Log SQLite query
      * Log method arguments
      * Intercept entire module
      * Dump memory segments
      * Memory scan
      * Stalker
      * Cpp Demangler
      * Early hook
    * Android
      * Binder transactions
      * Get system property
      * Reveal manually registered native symbols
      * Enumerate loaded classes
      * Class description
      * Turn WiFi off
      * Set proxy
      * Get IMEI
      * Hook io InputStream
      * Android make Toast
      * Await for specific module to load
      * Webview URLS
      * Print all runtime strings & stacktrace
      * String comparison
      * Hook JNI by address
      * Hook constructor
      * Hook Java reflection
      * Trace class
      * Hooking Unity3d
      * Get Android ID
      * Change location
      * Bypass FLAG_SECURE
      * Shared Preferences update
      * Hook all method overloads
      * Register broadcast receiver
      * Increase step count
      * File system access hook $ frida --codeshare FrenchYeti/android-file-system-access-hook -f com.example.app --no-pause
      * How to remove/disable java hooks ? Assign null to the implementation property.
    * iOS
      * OS Log
      * iOS alert box
      * File access
      * Observe class
      * Find application UUID
      * Extract cookies
      * Describe class members
      * Class hierarchy
      * Hook refelaction
      * Device properties
      * Take screenshot
      * Log SSH commands
    * Windows
    * Sublime snippets
    * Vim snippets
    * JEB
