# NativePointer

* `NativePointer`
  * 是什么：Frida中一个最基本的变量类型
  * 常简称：`ptr`
  * 官网文档
    * [NativePointer - JavaScript API](https://frida.re/docs/javascript-api/#nativepointer)

## NativePointer的函数=支持的操作

* `NativePointer`的函数=支持的操作
  * `new NativePointer(s)`：传入字符串，生成`NativePointer`
    * 大家常用=其简写为：`ptr(s)`
  * `isNull()`：判断是否为空
  * `add(rhs)`, `sub(rhs)`, `and(rhs)`, `or(rhs)`, `xor(rhs)`
  * `shr(n)`, `shl(n)`
  * `not()`
  * `sign([key, data])`
  * `strip([key])`
  * `blend(smallInteger)`
  * `equals(rhs)`
  * `compare(rhs)`
  * `toInt32()`
  * `toString([radix = 16])`
  * `toMatchPattern()`
  * `readPointer()`
  * `writePointer(ptr)`
  * `readS8()`, `readU8()`, `readS16()`, `readU16()`, `readS32()`, `readU32()`, `readShort()`, `readUShort()`, `readInt()`, `readUInt()`, `readFloat()`, `readDouble()`
  * `writeS8(value)`, `writeU8(value)`, `writeS16(value)`, `writeU16(value)`, `writeS32(value)`, `writeU32(value)`, `writeShort(value)`, `writeUShort(value)`, `writeInt(value)`, `writeUInt(value)`, `writeFloat(value)`, `writeDouble(value)`
  * `readS64()`, `readU64()`, `readLong()`, `readULong()`
  * `writeS64(value)`, `writeU64(value)`, `writeLong(value)`, `writeULong(value)`
  * `readByteArray(length)`
  * `writeByteArray(bytes)`
  * `readCString([size = -1])`, `readUtf8String([size = -1])`, `readUtf16String([length = -1])`, `readAnsiString([size = -1])`
  * `writeUtf8String(str)`, `writeUtf16String(str)`, `writeAnsiString(str)`

## NativePointer的典型用法

### 一些常见的写法

```js
myOffsetPtr = myBaseAddr.add(ptr('0x76E'))

console.log(hexdump(ptr(0x7cbe43acd0)))

Memory.protect(ptr('0x1234'), 4096, 'rw-');

ptr('0x7fff870135c9')
```

### onEnter中的args

frida中最常涉及到`NativePointer`的地方就是：`Interceptor.attach`的`onEnter`中的`args`，就是一个`NativePointer`的数组 = `an array of NativePointer objects`

[单个类的单个函数的代码举例](../../../frida_example/frida/ios_objc/hook_func/single_cls_single_func.md)中的：

```js
const curMethod = ObjC.classes.NSXPCConnection["- setExportedObject:"];

Interceptor.attach(curMethod.implementation, {
  onEnter: function(args) {
    const arg0 = args[0]
    const arg1 = args[1]
    const arg2 = args[2]
    console.log("args: Objc self id=" + arg0 + ", SEL=" + arg1 + ", objcRealArg1=" + arg2);
  }
});
```

中的`args`，就是NativePointer的数组，所以：`arg0`、`arg1`、`arg2`，都是`NativePointer`=`ptr`类型

而典型的后续的操作就是，去转换成对应的类型，去处理

比如：

* ObjC中，把传入的参数，转换成真正的ObjC的类
  ```js
  const argSelf = args[0];
  const argSelfObj = new ObjC.Object(argSelf);
  ```

### 用add计算函数地址

[Stalker实例代码](https://book.crifan.org/books/frida_advanced_debug_stalker/website/stalker_examples/) 中的

```js
var funcRelativeStartAddr = 0xa0460;
var functionSize = 0x24C8; // 9416 == 0x24C8
var funcRelativeEndAddr = funcRelativeStartAddr + functionSize;
const moduleName = "akd";
const moduleBaseAddress = Module.findBaseAddress(moduleName);
const funcRealStartAddr = moduleBaseAddress.add(funcRelativeStartAddr);
const funcRealEndAddr = funcRealStartAddr.add(functionSize);
```

中就是用到了`Module.findBaseAddress`的返回值，类型就是`NativePointer`，然后：

* `const funcRealStartAddr = moduleBaseAddress.add(funcRelativeStartAddr);`
  * 通过`add`加上起始地址，计算出函数真正的起始地址
* `const funcRealEndAddr = funcRealStartAddr.add(functionSize);`
  * 通过`add`加上函数大小，计算出真正的结束地址

输出的相关log：

```bash
funcRelativeStartAddr=656480, functionSize=9416, funcRelativeEndAddr=665896
moduleName=akd, moduleBaseAddress=0x10015c000
funcRealStartAddr=0x1001fc460, funcRealEndAddr=0x1001fe928
```

注意：

* 如果是`NativePointer`类型的变量，直接相加`+`普通的数值，则结果，不是数值上的相加，而是：数值的字符串拼接
  * 对比
    * 上述代码
      ```js
      const funcRealEndAddr = funcRealStartAddr.add(functionSize);
      ```
      * 结果：`funcRealEndAddr=0x1001fe928`
        * `= 0x1001fc460 + 0x24C8`
    * 如果改为
      ```js
      const funcRealEndAddr = funcRealStartAddr + functionSize;
      ```
      * 则结果是：`funcRealEndAddr=0x1001fc4609416`
        * = 地址的字符串`0x1001fc460` 拼接上 `9416`(=16进制的`0x24C8`的10进制值)
