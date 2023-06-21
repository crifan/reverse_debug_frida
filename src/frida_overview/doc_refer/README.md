# 文档和资料

* 官网文档入口: [Frida Docs](https://frida.re/docs/home/)
  * 入门指南=Getting Started
    * 快速入手: [Quick-start guide](https://frida.re/docs/quickstart/)
    * 安装: [Installation](https://frida.re/docs/installation/)
    * 操作模式: [Modes of Operation](https://frida.re/docs/modes/)
    * Gadget: [Gadget](https://frida.re/docs/gadget/)
    * 内部原理: [Hacking](https://frida.re/docs/hacking/)
    * Stalker: [Stalker](https://frida.re/docs/stalker/)
    * 演讲文档资料：[Presentations](https://frida.re/docs/presentations/)
  * 教程=Tutorials
    * 通用
      * 函数: [Functions](https://frida.re/docs/functions/)
      * 消息: [Messages](https://frida.re/docs/messages/)
    * 移动端
      * [iOS](https://frida.re/docs/ios/)
      * [Android](https://frida.re/docs/android/)
  * 案例=Examples
    * PC端
      * [Windows](https://frida.re/docs/examples/windows/)
      * [macOS](https://frida.re/docs/examples/macos/)
      * [Linux](https://frida.re/docs/examples/linux/)
    * 移动端
      * [iOS](https://frida.re/docs/examples/ios/)
      * [Android](https://frida.re/docs/examples/android/)
    * 其他
      * [JavaScript](https://frida.re/docs/examples/javascript/)
  * 工具=Tools
    * `frida`命令行工具: [Frida CLI](https://frida.re/docs/frida-cli/)
    * [frida-ps](https://frida.re/docs/frida-ps/)
    * [frida-trace](https://frida.re/docs/frida-trace/)
    * [frida-discover](https://frida.re/docs/frida-discover/)
    * [frida-ls-devices](https://frida.re/docs/frida-cli/)
    * [frida-kill](https://frida.re/docs/frida-kill/)
    * [gum-graft](https://frida.re/docs/gum-graft/)
  * API文档=API Reference
    * [JavaScript API](https://frida.re/docs/javascript-api/)
    * [C API](https://frida.re/docs/c-api/)
    * [Swift API](https://frida.re/docs/swift-api/)
    * [Go API](https://frida.re/docs/go-api/)
  * 其他细节=Miscellaneous
    * 最佳实践: [Best Practices](https://frida.re/docs/best-practices/)
    * 排错: [Troubleshooting](https://frida.re/docs/troubleshooting/)
    * 自己编译Frida=参与Frida开发: [Building](https://frida.re/docs/building/)
    * 占用系统空间: [Footprint](https://frida.re/docs/footprint/)
  * 起源和发展=Meta
    * [GSoC Ideas 2015](https://frida.re/docs/gsoc-ideas-2015/)
    * [GSoD Ideas 2023](https://frida.re/docs/gsod-ideas-2023/)
    * [History](https://frida.re/docs/history/)

## 其他一些Frida的使用案例

* [frida-presentations](https://github.com/frida/frida-presentations)
* [personal_script/Frida_script at master · lich4/personal_script · GitHub](https://github.com/lich4/personal_script/blob/master/Frida_script/antijailbreak.js)
* [Frida在iOS平台进行OC函数hook的常用方法 | 8Biiit's Blog](https://8biiit.github.io/2019/08/12/Frida/)
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
