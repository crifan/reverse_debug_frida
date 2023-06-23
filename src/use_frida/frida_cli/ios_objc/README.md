# Frida中iOS的ObjC

`Frida`支持hook调试`iOS`的`ObjC`的类和函数。

* Frida的ObjC
  * 官网文档
    * [Frida API - ObjC](https://frida.re/docs/javascript-api/#objc)
      * `ObjC.available`
      * `ObjC.api`
      * `ObjC.classes`
        * `ObjC.Object`
      * `ObjC.protocols`
        * `ObjC.Protocol`
      * `ObjC.mainQueue`
      * `ObjC.schedule(queue, work)`
      * `new ObjC.Object(handle[, protocol])`
      * `new ObjC.Protocol(handle)`
      * `new ObjC.Block(target[, options])`
      * `ObjC.implement(method, fn)`
      * `ObjC.registerProxy(properties)`
      * `ObjC.registerClass(properties)`
      * `ObjC.registerProtocol(properties)`
      * `ObjC.bind(obj, data)`
      * `ObjC.unbind(obj)`
      * `ObjC.getBoundData(obj)`
      * `ObjC.enumerateLoadedClasses([options, ]callbacks)`
      * `ObjC.enumerateLoadedClassesSync([options])`
      * `ObjC.choose(specifier, callbacks)`
      * `ObjC.chooseSync(specifier)`
      * `ObjC.selector(name)`
      * `ObjC.selectorAsString(sel)`
  * 常用工具类
    * [iOS Utils](https://codeshare.frida.re/@lichao890427/ios-utils/)

下面继续介绍，关于Frida中ObjC的基本逻辑：
