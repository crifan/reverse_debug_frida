# Frida概览

* `Frida`
  * 概述
    * 一款基于`python` + `javascript`的`hook框架`
    * `Android`、`iOS`的app逆向等领域中，最常用的工具之一
    * A world-class Dynamic instrumentation toolkit for developers, reverse-engineers, and security researchers
      * Inject JavaScript to observe and reprogram running programs on Windows, macOS, GNU/Linux, iOS, watchOS, tvOS, Android, FreeBSD, and QNX
  * 主要用法：iOS逆向期间，用`frida+js脚本`和`frida-trace`、`Frida`的`Stalker`等工具，去动态调试代码逻辑
  * 核心原理
    * 主要使用动态二进制插桩(DBI)技术
      * 将外部代码注入到现有的正在运行的二进制文件中，从而让它执行额外操作
        * 支持哪些额外操作
          * 访问进程内存
          * 在应用程序运行时覆盖函数
          * 从导入的类调用函数
          * 在堆上查找对象实例并使用
          * Hook、跟踪和拦截函数等
        * 注：调试器也能完成相应工作，不过非常麻烦，比如各种反调试
  * 功能和特点
    * Scriptable
      * Inject your own scripts into black box processes. Hook any function, spy on crypto APIs or trace private application code, no source code needed. Edit, hit save, and instantly see the results. All without compilation steps or program restarts.
    * Portable
      * 支持多种运行平台/系统：`Android`/`iOS`/`Linux`/`Win`/`macOS`等
        * Works on Windows, macOS, GNU/Linux, iOS, Android, and QNX. Install the Node.js bindings from npm, grab a Python package from PyPI, or use Frida through its Swift bindings, .NET bindings, Qt/Qml bindings, or C API.
    * Free
      * Frida is and will always be free software (free as in freedom). We want to empower the next generation of developer tools, and help other free software developers achieve interoperability through reverse engineering.
    * Battle-tested
      * We are proud that NowSecure is using Frida to do fast, deep analysis of mobile apps at scale. Frida has a comprehensive test-suite and has gone through years of rigorous testing across a broad range of use-cases.
  * 主页
    * 官网
      * https://frida.re/
        * 文档
          * https://frida.re/docs/home/
    * Github
      * https://github.com/frida/frida
  * 作者：`oleavr`=`Ole André Vadla Ravnås`
    * Github
      * https://github.com/oleavr
    * 所属公司
      * NowSecure
        * https://www.nowsecure.com/
