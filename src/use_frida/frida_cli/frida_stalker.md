# Frida的Stalker

* 官网文档
  * [Stalker介绍](https://frida.re/docs/stalker/)
  * [Stalker的API](https://frida.re/docs/javascript-api/#stalker)
* 概述
  * 核心逻辑
    * 在frida命令上，和普通frida一样，都是调用js
      ```bash
      frida -U -n akd -l ./fridaStalker_akdSymbol2575.js
      ```
    * js内部逻辑
      * 最初要初始化：计算出当前要hook的函数所属的模块和地址
      * 在普通的`Interceptor.attach`的`onEnter`中，加上`Stalker.follow`
        * 在`transform`中，计算是否是原始函数的代码
          * 如果是，再去：实现特定的调试的目的
            * 打印真实执行的指令的信息`instruction.toString()`
            * 打印当时的变量的值`context`
            * 等等
* 详解
  * 举例
    * [Stalker](../../frida_example/frida/ios_objc/stalker/README.md)
