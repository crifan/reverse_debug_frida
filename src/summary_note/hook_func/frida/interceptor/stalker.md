# Stalker

## 实际内存地址和Stalker输出地址不匹配不一致

某次Stalker调试的log是：

```bash
===== dynamicDebug/frida/scripts/fridaStalker_akdSymbol2575.js =====
funcRelativeStartAddr=656480, functionSize=9416, funcRelativeEndAddr=665896
moduleName=akd, moduleBaseAddress=0x10054c000
funcRealStartAddr=0x1005ec460, funcRealEndAddr=0x1005ec4609416
[iPhone::PID::13098 ]-> ----- arg0=0xfffffffffffffffe, arg1=0x16fe26838, arg2=0x16fe26838, arg3=0xfffffffffffffffe

curTid= 26635

+++ into iterator=
[0x1071dbcd8] b #0x1071dbce8
+++ into iterator=
[0x1071dbce8] str wzr, [x19, #0x90]
[0x1071dbcec] ldr x0, [x19, #0xa0]
[0x1071dbcf0] str xzr, [x19, #0xa0]
[0x1071dbcf4] cbz x0, #0x1071dbcfc
+++ into iterator=
[0x1071dbcf8] bl #0x1070dfef8
+++ into iterator=
[0x1071dbcfc] ldr x0, [x19, #0x98]
[0x1071dbd00] str xzr, [x19, #0x98]
[0x1071dbd04] cbz x0, #0x1071dbd14
...
```

其中指令地址看起来不匹配=不一致：

* 指令地址：0x1071dbcd8
* 函数二进制内偏移量：0xa0460
  * 此处akd模块基地址：0x10054c000
    * 所以函数的实际真实起始地址：0x1005ec460

后来才明白是：

Frida的Stalker中，看到2个函数地址不匹配：

* 函数实际内存起始地址：`0x1005ec460`
  * 0x1005exxxx 之类的地址
* Stalker中运行期间的函数地址：`0x1071dbcd8`
  * 0x1071dxxxx 之类的地址

的原因：

Stalker内部的hook指令的原理就是：把当前代码拷贝一份，加上hook代码及其他逻辑

而此处的：拷贝一份，到别处，别的一段内存地址空间，此处就是指的是：

* 0x1071dbcd8
  * 0x1071dxxxx 之类的地址

所以，原始函数代码和被Stalker去hook的代码，两个地址是不同的，是可以理解的。

## Frida的Stalker的transform中Instruction的属性

Frida的Stalker的transform中Instruction指令，有哪些属性，参考官网：

https://frida.re/docs/javascript-api/#instruction

得知有如下属性：

* `address`
* `next`
* `size`
* `mnemonic`
* `opStr`
* `operands`
* `regsAccessed`
* `regsRead`
* `regsWritten`
* `groups`
* `toString()`
* `toJSON()`

注：但是没有（其实希望也有的）`bytes`=`opcode`属性。

而这些属性的来源是：`Capstone`

* Capstone
  * Java接口
    * [capstone/Capstone.java at next · capstone-engine/capstone · GitHub](https://github.com/capstone-engine/capstone/blob/next/bindings/java/capstone/Capstone.java)
  * Python接口
    * [capstone/__init__.py at next · capstone-engine/capstone · GitHub](https://github.com/capstone-engine/capstone/blob/next/bindings/python/capstone/__init__.py)

### 举例

如之前示例代码：

[___lldb_unnamed_symbol2575$$akd](../../../../frida_example/frida/ios_objc/stalker/akd_lldb_unnamed_symbol2575.md)

中的，但是注释掉的代码：

（注：当时没加`regsAccessed`和`toJSON()`）

```js
    // console.log("instruction: address=" + instruction.address
    //     + ",next=" + instruction.next()
    //     + ",size=" + instruction.size
    //     + ",mnemonic=" + instruction.mnemonic
    //     + ",opStr=" + instruction.opStr
    //     + ",operands=" + JSON.stringify(instruction.operands)
    //     + ",regsAccessed=" + JSON.stringify(instruction.regsAccessed)
    //     + ",regsRead=" + JSON.stringify(instruction.regsRead)
    //     + ",regsWritten=" + JSON.stringify(instruction.regsWritten)
    //     + ",groups=" + JSON.stringify(instruction.groups)
    //     + ",toString()=" + instruction.toString()
    //     + ",toJSON()=" + instruction.toJSON()
    // );
```

取消注释后，可以输出log：

```bash
instruction: address=0x10f4ecef4,next=0x4,size=4,mnemonic=ldr,opStr=x0, #0x10f4ecf78,operands=[{"type":"reg","value":"x0","access":"w"},{"type":"imm","value":"4551790456","access":"r"}],regsRead=[],regsWritten=[],groups=[],toString()=ldr x0, #0x10f4ecf78
[0x10f4ecef4] ldr x0, #0x10f4ecf78

instruction: address=0x10f4ecef8,next=0x4,size=4,mnemonic=bl,opStr=#0x1091a500c,operands=[{"type":"imm","value":"4447686668","access":"r"}],regsRead=[],regsWritten=["lr"],groups=["call","jump","branch_relative"],toString()=bl #0x1091a500c
[0x10f4ecef8] bl #0x1091a500c
```

## Frida的Stalker中去判断是否是函数的代码指令的逻辑

* 需求：Frida的Stalker调试期间，想要知道当前所执行的代码，是否属于：真正的要hook的函数的真实代码
* 解决办法

经过调试用如下代码：

```js
console.log("===== Frida Stalker hook for arm64 ___lldb_unnamed_symbol2575$$akd =====");
// arm64 akd: ___lldb_unnamed_symbol2575$$akd
// var funcRelativeStartAddr = 0x1000a0460;
var funcRelativeStartAddr = 0xa0460;
var functionSize = 0x24C8; // 9416 == 0x24C8
var funcRelativeEndAddr = funcRelativeStartAddr + functionSize;
console.log("funcRelativeStartAddr=" + funcRelativeStartAddr + ", functionSize=" + functionSize + ", funcRelativeEndAddr=" + funcRelativeEndAddr);
const moduleName = "akd";
const moduleBaseAddress = Module.findBaseAddress(moduleName);
console.log("moduleName=" + moduleName + ", moduleBaseAddress=" + moduleBaseAddress);
// console.log("moduleName=%s, moduleBaseAddress=%p", moduleName, moduleBaseAddress);
const funcRealStartAddr = moduleBaseAddress.add(funcRelativeStartAddr);
// var funcRealEndAddr = funcRealStartAddr + functionSize;
const funcRealEndAddr = funcRealStartAddr.add(functionSize);
console.log("funcRealStartAddr=" + funcRealStartAddr + ", funcRealEndAddr=" + funcRealEndAddr);
var curTid = null;
Interceptor.attach(funcRealStartAddr, {
    onEnter: function(args) {
        var arg0 = args[0]
        var arg1 = args[1]
        var arg2 = args[2]
        var arg3 = args[3]
        console.log("----- arg0=" + arg0 + ", arg1=" + arg1 + ", arg2=" + arg2 + ", arg3=" + arg3);
        var curTid = Process.getCurrentThreadId();
        console.log("curTid=", curTid);
        Stalker.follow(curTid, {
            events: {
                call: true, // CALL instructions: yes please            
                ret: false, // RET instructions
                exec: false, // all instructions: not recommended as it's
                block: false, // block executed: coarse execution trace
                compile: false // block compiled: useful for coverage
            },
            // transform: (iterator: StalkerArm64Iterator) => {
            transform: function (iterator) {
                var instruction = iterator.next();
                const startAddress = instruction.address;
                console.log("+++ into iterator: startAddress=" + startAddress);
                // const isAppCode = startAddress.compare(funcRealStartAddr) >= 0 && startAddress.compare(funcRealEndAddr) === -1;
                // const isAppCode = (startAddress.compare(funcRealStartAddr) >= 0) && (startAddress.compare(funcRealEndAddr) < 0);
                const gt_realStartAddr = startAddress.compare(funcRealStartAddr) >= 0
                const lt_realEndAddr = startAddress.compare(funcRealEndAddr) < 0
                const isAppCode = gt_realStartAddr && lt_realEndAddr
                console.log("isAppCode=" + isAppCode + ", gt_realStartAddr=" + gt_realStartAddr + ", lt_realEndAddr=" + lt_realEndAddr);
                do {
                    if (isAppCode) {
                        var curRealAddr = instruction.address;
                        var curOffset = curRealAddr.sub(funcRealStartAddr)
                        var curOffsetInt = curOffset.toInt32()
                        var instructionStr = instruction.toString()
                        console.log("\t" + curRealAddr + " <+" + curOffsetInt + ">: " + instructionStr);
                    }
                    iterator.keep();
                } while ((instruction = iterator.next()) !== null);
             }
        })
    },
    onLeave: function(retval) {
        console.log("retval:", new ObjC.Object(retval))
        if (curTid != null) {
            Stalker.unfollow(curTid);
            console.log("Stalker.unfollow curTid=", curTid)
        }
    }
});
```

可以输出：

```bash
===== Frida Stalker hook for arm64 ___lldb_unnamed_symbol2575$$akd =====
funcRelativeStartAddr=656480, functionSize=9416, funcRelativeEndAddr=665896
moduleName=akd, moduleBaseAddress=0x10237c000
funcRealStartAddr=0x10241c460, funcRealEndAddr=0x10241e928
[iPhone::akd ]-> ----- arg0=0xfffffffffffffffe, arg1=0x16dcae838, arg2=0x16dcae838, arg3=0xfffffffffffffffe
curTid= 15635
+++ into iterator: startAddress=0x1089dbcd8
isAppCode=false, gt_realStartAddr=true, lt_realEndAddr=false
+++ into iterator: startAddress=0x1089dbce8
...
isAppCode=true, gt_realStartAddr=true, lt_realEndAddr=true, arg3=0xfffffffffffffffe
    0x10241c470 <+16>: stp x22, x21, [sp, #0xc0]
    0x10241c474 <+20>: stp x20, x19, [sp, #0xd0]
    0x10241c478 <+24>: stp x29, x30, [sp, #0xe0]
    0x10241c47c <+28>: add x29, sp, #0xe0
...
```

算是实现了，通过计算，最后用`isAppCode`这个变量去判断是否是当前代码

上述代码的所属的完整的示例代码，详见：

[___lldb_unnamed_symbol2575$$akd](../../../../frida_example/frida/ios_objc/stalker/akd_lldb_unnamed_symbol2575.md)

