# 数据类型

Frida中典型的数据类型有：

* `Int64`
  * https://frida.re/docs/javascript-api/#int64
* `UInt64`
  * https://frida.re/docs/javascript-api/#uint64
* [NativePointer](../../../use_frida/frida_cli/data_type/nativepointer.md)
* `ArrayBuffer`
  * https://frida.re/docs/javascript-api/#arraybuffer
* 其他相关
  * Function函数
    * `NativeFunction`
      * https://frida.re/docs/javascript-api/#nativefunction
    * `SystemFunction`
      * https://frida.re/docs/javascript-api/#systemfunction
  * Callback回调
    * `NativeCallback`
      * https://frida.re/docs/javascript-api/#nativecallback

## 常见缩写

和数据类型有关的，常见缩写有：

* `int64(v)` == `new Int64(v)`
* `uint64(v)` == `new UInt64(v)`
* `ptr(s)` == `new NativePointer(s)`
* `NULL` == `ptr("0")`
