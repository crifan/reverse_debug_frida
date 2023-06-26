# console.log

## 问题

### console.log不支持参数的格式化

* 问题
  * Frida中console.log打印参数，去格式化参数：
    ```js
    console.log("moduleName=%s, moduleBaseAddress=%p", moduleName, moduleBaseAddress)
    ```
    * 输出：
    ```bash
    moduleName=%s, moduleBaseAddress=%p akd 0x10015c000
    ```
* 结论：Frida中js的console.log，不支持参数格式化 == `%s`、`%d`、`%o`、`%p`等格式化参数无效
* 规避办法
  * 用`逗号`=`,` 或 `加号`=`+` 去打印参数
    ```js
    console.log("moduleName=", moduleName, ", moduleBaseAddress=", moduleBaseAddress)

    console.log("moduleName=" + moduleName + ", moduleBaseAddress=" + moduleBaseAddress)
    ```

## 心得

## 用`JSON.stringify`打印对象

既然console.log中无法直接（用`%o`去格式化）打印对象，那么可以考虑用`JSON.stringify`去转化为JSON再去打印：

举例：

```js
console.log("instruction: address=" + instruction.address
    + ",next=" + instruction.next
    + ",size=" + instruction.size
    + ",mnemonic=" + instruction.mnemonic
    + ",opStr=" + instruction.opStr
    + ",operands=" + JSON.stringify(instruction.operands)
    + ",regsRead=" + JSON.stringify(instruction.regsRead)
    + ",regsWritten=" + JSON.stringify(instruction.regsWritten)
    + ",groups=" + JSON.stringify(instruction.groups)
    + ",toString()=" + instruction.toString()
);
```

输出效果：

```bash
instruction: address=0x1091dbcd8,next=0x4,size=4,mnemonic=b,opStr=#0x1091dbce8,operands=[{"type":"imm","value":"4447911144","access":"r"}],regsRead=[],regsWritten=[],groups=["jump","branch_relative"],toString()=b #0x1091dbce8
[0x1091dbcd8] b #0x1091dbce8
+++ into iterator=
instruction: address=0x1091dbce8,next=0x4,size=4,mnemonic=str,opStr=wzr, [x19, #0x90],operands=[{"type":"reg","value":"wzr","access":"r"},{"type":"mem","value":{"base":"x19","disp":144},"access":"rw"}],regsRead=[],regsWritten=[],groups=[],toString()=str wzr, [x19, #0x90]
[0x1091dbce8] str wzr, [x19, #0x90]
```

## 让console.log输出日志到文件

* 需求：希望Frida中console.log打印的日志，不是输出到当前终端，而是输出到文件中
  * 目的：方便后续随时查看，方便调试
* 解决办法：

在运行Frida带js的命令行最后加上：

* `> xxx.log 2>&1`
  * 参数说明
    * `> xxx.log`：把普通的=`stdout`日志信息，都输出到`log文件`
    * `2>&1`：把特殊的`stderr`=错误日志信息，输出到终端
* 举例
  ```bash
  frida -U -p 13098 -l ./fridaStalker_akdSymbol2575.js > stalker_akd1575.log 2>&1
  ```

## Frida的Stalker中优化hook到指令时的log日志输出

* 需求：Frida的Stalker中，希望输出的log日志，尽量也模拟之前Xcode的汇编代码的形式
* 核心代码：
  ```js
    var curRealAddr = instruction.address;
    var curOffsetHexPtr = curRealAddr.sub(funcRealStartAddr)
    var curOffsetInt = curOffsetHexPtr.toInt32()
    var instructionStr = instruction.toString()
    console.log("\t" + curRealAddr + " <+" + curOffsetInt + ">: " + instructionStr);
  ```
* 输出效果
```bash
+++ into iterator: startAddress=0x104b48470
    0x104b48470 <+16>: stp x22, x21, [sp, #0xc0]
    0x104b48474 <+20>: stp x20, x19, [sp, #0xd0]
    0x104b48478 <+24>: stp x29, x30, [sp, #0xe0]
    0x104b4847c <+28>: add x29, sp, #0xe0
    0x104b48480 <+32>: nop
    0x104b48484 <+36>: ldr x8, #0x104b9c7d8
```
  * 注：完整代码和输出，详见：[___lldb_unnamed_symbol2575$$akd](../../frida_example/frida/ios_objc/stalker/akd_lldb_unnamed_symbol2575.md)
