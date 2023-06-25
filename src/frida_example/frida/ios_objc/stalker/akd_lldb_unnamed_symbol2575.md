# ___lldb_unnamed_symbol2575$$akd

用`Frida`的`Stalker`调试`arm64`的`akd`函数`___lldb_unnamed_symbol2575$$akd`：

### Frida命令

```bash
frida -U -n akd -l ./fridaStalker_akdSymbol2575.js
```

### js文件`fridaStalker_akdSymbol2575.js`的内容

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
                call: false, // CALL instructions: yes please            
                ret: true, // RET instructions
                exec: false, // all instructions: not recommended as it's
                block: false, // block executed: coarse execution trace
                compile: false // block compiled: useful for coverage
            },
            // onReceive: Called with `events` containing a binary blob comprised of one or more GumEvent structs. See `gumevent.h` for details about the format. Use `Stalker.parse()` to examine the data.
            onReceive(events) {
                var parsedEvents = Stalker.parse(events)
                // var parsedEventsStr = JSON.stringify(parsedEventsStr)
                // console.log(">>> into onReceive: parsedEvents=" + parsedEvents + ", parsedEventsStr=" + parsedEventsStr);
                console.log(">>> into onReceive: parsedEvents=" + parsedEvents);
            },
    
            // transform: (iterator: StalkerArm64Iterator) => {
            transform: function (iterator) {
                // console.log("iterator=" + iterator);
                var instruction = iterator.next();
                const startAddress = instruction.address;
                // console.log("+++ into iterator: startAddress=" + startAddress);
                // const isAppCode = startAddress.compare(funcRealStartAddr) >= 0 && startAddress.compare(funcRealEndAddr) === -1;
                // const isAppCode = (startAddress.compare(funcRealStartAddr) >= 0) && (startAddress.compare(funcRealEndAddr) < 0);
                const gt_realStartAddr = startAddress.compare(funcRealStartAddr) >= 0
                const lt_realEndAddr = startAddress.compare(funcRealEndAddr) < 0
                var isAppCode = gt_realStartAddr && lt_realEndAddr
                console.log("+++ into iterator: startAddress=" + startAddress + ", isAppCode=" + isAppCode);

                // // for debug
                // isAppCode = true

                // console.log("isAppCode=" + isAppCode + ", gt_realStartAddr=" + gt_realStartAddr + ", lt_realEndAddr=" + lt_realEndAddr);
                do {
                    if (isAppCode) {
                        // is origal function code = which we focus on

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

                        var curRealAddr = instruction.address;
                        // const isAppCode = curRealAddr.compare(funcRealStartAddr) >= 0 && curRealAddr.compare(funcRealEndAddr) === -1;
                        // console.log(curRealAddr + ": isAppCode=" + isAppCode);
                        var curOffsetHexPtr = curRealAddr.sub(funcRealStartAddr)
                        var curOffsetInt = curOffsetHexPtr.toInt32()
      
                        // var instructionStr = instruction.mnemonic + " " + instruction.opStr
                        var instructionStr = instruction.toString()
                        // console.log("\t" + curRealAddr + ": " + instructionStr);
                        // console.log("\t" + curRealAddr + " <+" + curOffsetHexPtr + ">: " + instructionStr);
                        console.log("\t" + curRealAddr + " <+" + curOffsetInt + ">: " + instructionStr);
                        if (curOffsetInt == 8516) {
                            console.log("curOffsetInt=" + curOffsetInt);
                            // iterator.putCallout(needDebug);
                            iterator.putCallout((context) => {
                                var contextStr = JSON.stringify(context)
                                console.log("contextStr=" + contextStr);
                                var x9Value1 = context.x9
                                var x9Value2 = context["x9"]
                                console.log("x9Value1=" + x9Value1 + ", x9Value2=" + x9Value2)
                            });
                        }
                    }
                    iterator.keep();
                } while ((instruction = iterator.next()) !== null);
            }
        });

        // function needDebug(context) {
        //     console.log("into needDebug");
        //     // console.log("into needDebug: context=" + context);
        //     // var contextStr = JSON.stringify(context, null, 2)
        //     // console.log("context=" + contextStr);
        //     // var x9Value1 = context.x9
        //     // var x9Value2 = context["x9"]
        //     // console.log("x9Value1=" + x9Value1 + ", x9Value2=" + x9Value2)
        // }
    },
    onLeave: function(retval) {
        console.log("retval=", new ObjC.Object(retval))
        if (curTid != null) {
            Stalker.unfollow(curTid);
            console.log("Stalker.unfollow curTid=", curTid)
        }
    }
});
```

### 输出

最开始输出环境初始化信息：

```log
===== Frida Stalker hook for arm64 ___lldb_unnamed_symbol2575$$akd =====
funcRelativeStartAddr=656480, functionSize=9416, funcRelativeEndAddr=665896
moduleName=akd, moduleBaseAddress=0x1040e4000
funcRealStartAddr=0x104184460, funcRealEndAddr=0x104186928
[iPhone::akd ]-> ----- arg0=0xfffffffffffffffe, arg1=0x16bf46838, arg2=0x16bf46838, arg3=0xfffffffffffffffe
curTid= 9739
```

（如果开启调试：

```js
// for debug
isAppCode = true
```

则会输出）

（最初是）一些非原始函数代码：

```log
+++ into iterator: startAddress=0x106d0bcd8, isAppCode=false
    0x106d0bcd8 <+45643896>: b #0x106d0bce8
+++ into iterator: startAddress=0x106d0bce8, isAppCode=false
    0x106d0bce8 <+45643912>: str wzr, [x19, #0x90]
    0x106d0bcec <+45643916>: ldr x0, [x19, #0xa0]
    0x106d0bcf0 <+45643920>: str xzr, [x19, #0xa0]
    0x106d0bcf4 <+45643924>: cbz x0, #0x106d0bcfc
+++ into iterator: startAddress=0x106d0bcf8, isAppCode=false
    0x106d0bcf8 <+45643928>: bl #0x106c0fef8
+++ into iterator: startAddress=0x106d0bcfc, isAppCode=false
    0x106d0bcfc <+45643932>: ldr x0, [x19, #0x98]
    0x106d0bd00 <+45643936>: str xzr, [x19, #0x98]
    0x106d0bd04 <+45643940>: cbz x0, #0x106d0bd14
+++ into iterator: startAddress=0x106d0bd08, isAppCode=false
    0x106d0bd08 <+45643944>: ldp x29, x30, [sp, #0x10]
    0x106d0bd0c <+45643948>: ldp x20, x19, [sp], #0x20
    0x106d0bd10 <+45643952>: b #0x106c0fef8
+++ into iterator: startAddress=0x106c0fef8, isAppCode=false
    0x106c0fef8 <+44612248>: stp x24, x23, [sp, #-0x40]!
    0x106c0fefc <+44612252>: stp x22, x21, [sp, #0x10]
    0x106c0ff00 <+44612256>: stp x20, x19, [sp, #0x20]
    0x106c0ff04 <+44612260>: stp x29, x30, [sp, #0x30]
    0x106c0ff08 <+44612264>: add x29, sp, #0x30
    0x106c0ff0c <+44612268>: mov x19, x0
    0x106c0ff10 <+44612272>: add x23, x0, #8
    0x106c0ff14 <+44612276>: add x20, x0, #0x10
    0x106c0ff18 <+44612280>: adrp x22, #0x107d28000
    0x106c0ff1c <+44612284>: adrp x21, #0x107d28000
    0x106c0ff20 <+44612288>: add x21, x21, #0x790
    0x106c0ff24 <+44612292>: ldar w24, [x23]
    0x106c0ff28 <+44612296>: cmp w24, #2
    0x106c0ff2c <+44612300>: b.lt #0x106c0ff50
+++ into iterator: startAddress=0x106c0ff30, isAppCode=false
    0x106c0ff30 <+44612304>: bl #0x106c12110
+++ into iterator: startAddress=0x106c0ff34, isAppCode=false
    0x106c0ff34 <+44612308>: sub w8, w24, #1
    0x106c0ff38 <+44612312>: ldaxr w9, [x23]
    0x106c0ff3c <+44612316>: cmp w9, w24
    0x106c0ff40 <+44612320>: b.ne #0x106c0ff7c
    0x106c0ff44 <+44612324>: stlxr w9, w8, [x23]
    0x106c0ff48 <+44612328>: cbnz w9, #0x106c0ff38
+++ into iterator: startAddress=0x106c0ff4c, isAppCode=false
    0x106c0ff4c <+44612332>: b #0x106c0ff84
+++ into iterator: startAddress=0x106c0ff84, isAppCode=false
    0x106c0ff84 <+44612388>: cmp w24, #2
    0x106c0ff88 <+44612392>: b.ne #0x106c10028
+++ into iterator: startAddress=0x106c0ff8c, isAppCode=false
    0x106c0ff8c <+44612396>: tbz w0, #0, #0x106c10028
+++ into iterator: startAddress=0x106c10028, isAppCode=false
    0x106c10028 <+44612552>: ldp x29, x30, [sp, #0x30]
    0x106c1002c <+44612556>: ldp x20, x19, [sp, #0x20]
    0x106c10030 <+44612560>: ldp x22, x21, [sp, #0x10]
    0x106c10034 <+44612564>: ldp x24, x23, [sp], #0x40
    0x106c10038 <+44612568>: ret
+++ into iterator: startAddress=0x106d0a104, isAppCode=false
    0x106d0a104 <+45636772>: ldur x8, [x29, #-0x48]
    0x106d0a108 <+45636776>: adrp x9, #0x107c28000
    0x106d0a10c <+45636780>: ldr x9, [x9, #0x28]
    0x106d0a110 <+45636784>: ldr x9, [x9]
    0x106d0a114 <+45636788>: cmp x9, x8
    0x106d0a118 <+45636792>: b.ne #0x106d0a138
+++ into iterator: startAddress=0x106d0a11c, isAppCode=false
    0x106d0a11c <+45636796>: ldp x29, x30, [sp, #0x100]
    0x106d0a120 <+45636800>: ldp x20, x19, [sp, #0xf0]
    0x106d0a124 <+45636804>: ldp x22, x21, [sp, #0xe0]
    0x106d0a128 <+45636808>: ldp x24, x23, [sp, #0xd0]
    0x106d0a12c <+45636812>: ldp x28, x27, [sp, #0xc0]
    0x106d0a130 <+45636816>: add sp, sp, #0x110
    0x106d0a134 <+45636820>: ret
+++ into iterator: startAddress=0x106cb825c, isAppCode=false
    0x106cb825c <+45301244>: add w22, w22, #1
    0x106cb8260 <+45301248>: b #0x106cb820c
+++ into iterator: startAddress=0x106cb820c, isAppCode=false
    0x106cb820c <+45301164>: ldr w8, [x25, #8]
    0x106cb8210 <+45301168>: cmp w22, w8
    0x106cb8214 <+45301172>: b.eq #0x106cb8264
+++ into iterator: startAddress=0x106cb8264, isAppCode=false
    0x106cb8264 <+45301252>: ldr w25, [x26, #0x10]
    0x106cb8268 <+45301256>: tbnz w19, #0, #0x106cb8274
+++ into iterator: startAddress=0x106cb8274, isAppCode=false
    0x106cb8274 <+45301268>: ldr x21, [sp, #8]
    0x106cb8278 <+45301272>: mov x0, x25
    0x106cb827c <+45301276>: bl #0x106cc899c
+++ into iterator: startAddress=0x106cb8280, isAppCode=false
    0x106cb8280 <+45301280>: adrp x8, #0x107d2a000
    0x106cb8284 <+45301284>: ldr x0, [x8, #0x780]
    0x106cb8288 <+45301288>: bl #0x106cb8b94
+++ into iterator: startAddress=0x106cb828c, isAppCode=false
    0x106cb828c <+45301292>: cbz w19, #0x106cb829c
+++ into iterator: startAddress=0x106cb8290, isAppCode=false
    0x106cb8290 <+45301296>: ldr x8, [x20, #0x70]
    0x106cb8294 <+45301300>: ldr x9, [sp, #0x10]
    0x106cb8298 <+45301304>: str x8, [x9]
    0x106cb829c <+45301308>: ldr x8, [x27]
    0x106cb82a0 <+45301312>: cbz x8, #0x106cb82dc
+++ into iterator: startAddress=0x106cb82dc, isAppCode=false
    0x106cb82dc <+45301372>: add x27, x20, #0x68
    0x106cb82e0 <+45301376>: ldr x8, [x27]
    0x106cb82e4 <+45301380>: str x8, [x21]
    0x106cb82e8 <+45301384>: tbnz w19, #0, #0x106cb82fc
+++ into iterator: startAddress=0x106cb82fc, isAppCode=false
    0x106cb82fc <+45301404>: ldp x29, x30, [sp, #0x90]
    0x106cb8300 <+45301408>: ldp x20, x19, [sp, #0x80]
    0x106cb8304 <+45301412>: ldp x22, x21, [sp, #0x70]
    0x106cb8308 <+45301416>: ldp x24, x23, [sp, #0x60]
    0x106cb830c <+45301420>: ldp x26, x25, [sp, #0x50]
    0x106cb8310 <+45301424>: ldp x28, x27, [sp, #0x40]
    0x106cb8314 <+45301428>: add sp, sp, #0xa0
    0x106cb8318 <+45301432>: ret
+++ into iterator: startAddress=0x1044fc0b0, isAppCode=false
    0x1044fc0b0 <+3636304>: add sp, sp, #0x10
    0x1044fc0b4 <+3636308>: ldp x1, x0, [sp], #0x10
    0x1044fc0b8 <+3636312>: msr nzcv, x1
    0x1044fc0bc <+3636316>: ldp x1, x2, [sp], #0x10
    0x1044fc0c0 <+3636320>: ldp x3, x4, [sp], #0x10
    0x1044fc0c4 <+3636324>: ldp x5, x6, [sp], #0x10
    0x1044fc0c8 <+3636328>: ldp x7, x8, [sp], #0x10
    0x1044fc0cc <+3636332>: ldp x9, x10, [sp], #0x10
    0x1044fc0d0 <+3636336>: ldp x11, x12, [sp], #0x10
    0x1044fc0d4 <+3636340>: ldp x13, x14, [sp], #0x10
    0x1044fc0d8 <+3636344>: ldp x15, x16, [sp], #0x10
    0x1044fc0dc <+3636348>: ldp x17, x18, [sp], #0x10
    0x1044fc0e0 <+3636352>: ldp x19, x20, [sp], #0x10
    0x1044fc0e4 <+3636356>: ldp x21, x22, [sp], #0x10
    0x1044fc0e8 <+3636360>: ldp x23, x24, [sp], #0x10
    0x1044fc0ec <+3636364>: ldp x25, x26, [sp], #0x10
    0x1044fc0f0 <+3636368>: ldp x27, x28, [sp], #0x10
    0x1044fc0f4 <+3636372>: ldp x29, x30, [sp], #0x10
    0x1044fc0f8 <+3636376>: ldp q0, q1, [sp], #0x20
    0x1044fc0fc <+3636380>: ldp q2, q3, [sp], #0x20
    0x1044fc100 <+3636384>: ldp q4, q5, [sp], #0x20
    0x1044fc104 <+3636388>: ldp q6, q7, [sp], #0x20
    0x1044fc108 <+3636392>: ldp q8, q9, [sp], #0x20
    0x1044fc10c <+3636396>: ldp q10, q11, [sp], #0x20
    0x1044fc110 <+3636400>: ldp q12, q13, [sp], #0x20
    0x1044fc114 <+3636404>: ldp q14, q15, [sp], #0x20
    0x1044fc118 <+3636408>: ldp q16, q17, [sp], #0x20
    0x1044fc11c <+3636412>: ldp q18, q19, [sp], #0x20
    0x1044fc120 <+3636416>: ldp q20, q21, [sp], #0x20
    0x1044fc124 <+3636420>: ldp q22, q23, [sp], #0x20
    0x1044fc128 <+3636424>: ldp q24, q25, [sp], #0x20
    0x1044fc12c <+3636428>: ldp q26, q27, [sp], #0x20
    0x1044fc130 <+3636432>: ldp q28, q29, [sp], #0x20
    0x1044fc134 <+3636436>: ldp q30, q31, [sp], #0x20
    0x1044fc138 <+3636440>: ldp x16, x17, [sp], #0x10
    0x1044fc13c <+3636444>: ret x16
+++ into iterator: startAddress=0x108040030, isAppCode=false
    0x108040030 <+65780688>: sub sp, sp, #0xf0
    0x108040034 <+65780692>: stp x28, x27, [sp, #0x90]
    0x108040038 <+65780696>: stp x26, x25, [sp, #0xa0]
    0x10804003c <+65780700>: stp x24, x23, [sp, #0xb0]
    0x108040040 <+65780704>: ldr x16, #0x108040048
    0x108040044 <+65780708>: br x16
```

之后才是：原始函数代码

```log
+++ into iterator: startAddress=0x104184470, isAppCode=true
    0x104184470 <+16>: stp x22, x21, [sp, #0xc0]
    0x104184474 <+20>: stp x20, x19, [sp, #0xd0]
    0x104184478 <+24>: stp x29, x30, [sp, #0xe0]
    0x10418447c <+28>: add x29, sp, #0xe0
    0x104184480 <+32>: nop
    0x104184484 <+36>: ldr x8, #0x1041d87d8
    0x104184488 <+40>: ldr x8, [x8]
    0x10418448c <+44>: stur x8, [x29, #-0x58]
    0x104184490 <+48>: mov w26, #0x5a87
    0x104184494 <+52>: movk w26, #0x6c24, lsl #16
    0x104184498 <+56>: add x8, x0, #6
    0x10418449c <+60>: cmp x8, #5
    0x1041844a0 <+64>: ccmn x0, #8, #4, hs
    0x1041844a4 <+68>: mov w8, #1
    0x1041844a8 <+72>: csel w8, wzr, w8, eq
    0x1041844ac <+76>: mov w11, #0xa5a2
    0x1041844b0 <+80>: movk w11, #0x93db, lsl #16
    0x1041844b4 <+84>: cmp x1, #0
    0x1041844b8 <+88>: cset w9, eq
    0x1041844bc <+92>: orr w8, w9, w8
    0x1041844c0 <+96>: add w9, w26, w8
    0x1041844c4 <+100>: add w9, w9, w11
    0x1041844c8 <+104>: sub w9, w9, #0x21
    0x1041844cc <+108>: adr x25, #0x1041a6cb0
    0x1041844d0 <+112>: nop
    0x1041844d4 <+116>: ldrsw x9, [x25, w9, sxtw #2]
    0x1041844d8 <+120>: nop
    0x1041844dc <+124>: ldr x10, #0x1041dd158
    0x1041844e0 <+128>: add x9, x9, x10
    0x1041844e4 <+132>: mov w22, #-0xafc9
    0x1041844e8 <+136>: br x9
+++ into iterator: startAddress=0x1041844ec, isAppCode=true
    0x1041844ec <+140>: mov x23, x1
    0x1041844f0 <+144>: mov x28, x0
    0x1041844f4 <+148>: eor w8, w8, #1
    0x1041844f8 <+152>: sub w9, w11, #0x29
    0x1041844fc <+156>: madd w22, w8, w9, w26
    0x104184500 <+160>: mov x20, #0xa930
    0x104184504 <+164>: movk x20, #0xea59, lsl #16
    0x104184508 <+168>: movk x20, #0x9bdd, lsl #32
    0x10418450c <+172>: movk x20, #0xd570, lsl #48
    0x104184510 <+176>: add w8, w22, #0xb
    0x104184514 <+180>: adr x24, #0x1041ddfe0
    0x104184518 <+184>: nop
    0x10418451c <+188>: ldr x8, [x24, w8, sxtw #3]
    0x104184520 <+192>: sub x8, x8, #2
    0x104184524 <+196>: mov w0, #0x18
    0x104184528 <+200>: str x8, [sp, #0x60]
    0x10418452c <+204>: blr x8
```

其中触发了`onReceive`，对应输出的log是：

```bash
+++ into iterator: startAddress=0x194d31f44, isAppCode=false
+++ into iterator: startAddress=0x102e327ec, isAppCode=true
    0x102e327ec <+9100>: mov w8, #0x6beb
    0x102e327f0 <+9104>: movk w8, #0x3920, lsl #16
    0x102e327f4 <+9108>: add w24, w21, w23
    0x102e327f8 <+9112>: str xzr, [x20]
    0x102e327fc <+9116>: str w8, [x20, #8]
    0x102e32800 <+9120>: mov w8, #0xf28c
    0x102e32804 <+9124>: movk w8, #0x3820, lsl #16
    0x102e32808 <+9128>: str w8, [x20, #0xc]
    0x102e3280c <+9132>: mov x8, #0x77e8
    0x102e32810 <+9136>: movk x8, #0xcfa6, lsl #16
    0x102e32814 <+9140>: movk x8, #0xde0b, lsl #32
    0x102e32818 <+9144>: movk x8, #0x20cd, lsl #48
    0x102e3281c <+9148>: add x0, x19, x8
    0x102e32820 <+9152>: blr x27
+++ into iterator: startAddress=0x194d30564, isAppCode=false
>>> into onReceive: parsedEvents=ret,0x1084e0038,0x1085da104,0,ret,0x1085da134,0x10858825c,-1,ret,0x108588318,0x1036540b0,-2,ret,0x10365413c,0x10984000c,-3,ret,0x1ddc2abc0,0x194d2e05c,0,ret,0x194d2fbdc,0x194d2dc74,1,ret,0x194d311c4,0x194d2d830,1,ret,0x194d2d898,0x194d2e0a0,0,ret,0x1ddc2ac00,0x194d2e374,0,ret,0x194d2e3a8,0x194d31214,-1,ret,0x194d312e8,0x194d30478,-2,ret,0x194d304a4,0x102e30530,-3,ret,0x1ddc2abc0,0x194d2cd44,0,ret,0x194d31894,0x194d2f0a0,1,ret,0x194d31b3c,0x194d2f0c0,1,ret,0x194d2f1ac,0x194d2cd88,0,ret,0x1ddc2ac00,0x194d2d280,0,ret,0x194d2d2b4,0x194d3123c,-1,ret,0x194d312e8,0x194d30478,-2,ret,0x194d304a4,0x102e305ac,-3,ret,0x1bd80959c,0x102e244e0,-2,ret,0x1ddc36aec,0x183f794d8,0,ret,0x183f77ab0,0x183f7958c,0,ret,0x1bd808ca8,0x1bd809c04,1,ret,0x1bd809c40,0x183f795a0,0,ret,0x183f7970c,0x183f795e4,0,ret,0x183f7963c,0x183f7aa34,-1,ret,0x183f7aa98,0x102e24508,-2,ret,0x1bd808b70,0x1bd809194,-1,ret,0x1bd809240,0x102e24528,-2,ret,0x102e24630,0x102e315c4,-3,ret,0x1ddc2abc0,0x194d2e05c,0,ret,0x194d311c4,0x194d2d830,1,ret,0x194d2d898,0x194d2e0a0,0,ret,0x1ddc2ac00,0x194d2e374,0,ret,0x194d2e3a8,0x194d31214,-1,ret,0x194d312e8,0x194d30478,-2,ret,0x194d304a4,0x102e316b8,-3,ret,0x1ddc2abc0,0x194d2cd44,0,ret,0x194d31894,0x194d2efe4,1,ret,0x194d2f1ac,0x194d2cd88,0,ret,0x1ddc2ac00,0x194d2d280,0,ret,0x194d2d2b4,0x194d3123c,-1,ret,0x194d312e8,0x194d30478,-2,ret,0x194d304a4,0x102e31744,-3,ret,0x1bd808aa4,0x1bd809b78,-2,ret,0x1bd809bb0,0x102e32168,-3,ret,0x194d305c4,0x194d30a34,-1,ret,0x194d33380,0x194d30a50,-1,ret,0x194d30a1c,0x194d2c188,-2,ret,0x1ddc2abc0,0x194d31c80,-2,ret,0x194d2c06c,0x194d31dec,-2,ret,0x194d31894,0x194d31e00,-2,ret,0x194d31b3c,0x194d31eec,-2,ret,0x194d30e90,0x194d31f34,-2,ret,0x1ddc2ac00,0x102e327ec,-3
+++ into iterator: startAddress=0x194d30584, isAppCode=false
+++ into iterator: startAddress=0x194d30588, isAppCode=false
```

再往后，就是：非原始函数代码，原始函数代码 交替输出

而我们此处只关心 原始函数代码，去调试输出即可。

比如最后此刻所调试的`+8516`行的代码

```log
+++ into iterator: startAddress=0x1041865a0, isAppCode=true
    0x1041865a0 <+8512>: mov w8, #0
    0x1041865a4 <+8516>: str x9, [x10]
curOffsetInt=8516
    0x1041865a8 <+8520>: b #0x104186740
contextStr={"pc":"0x1041865a4","sp":"0x16bf46740","nzcv":1610612736,"x0":"0x0","x1":"0x104ba0000","x2":"0xc","x3":"0x29","x4":"0x470b","x5":"0x0","x6":"0x0","x7":"0xec0","x8":"0x0","x9":"0x3050500","x10":"0x16bf46838","x11":"0x6c245a87","x12":"0x6c245a3d","x13":"0x1041864dc","x14":"0x45","x15":"0x0","x16":"0xd5709bdf0d7a48f0","x17":"0x123209fc0","x18":"0x0","x19":"0xdf3221f55389ecc8","x20":"0x1233064b0","x21":"0x93dba5a2","x22":"0x0","x23":"0x39207beb","x24":"0x6c245a87","x25":"0x1041a6cb0","x26":"0x6c245a87","x27":"0x194d2c100","x28":"0xfffffffffffffffe","fp":"0x16bf46820","lr":"0x104186168","q0":{},"q1":{},"q2":{},"q3":{},"q4":{},"q5":{},"q6":{},"q7":{},"q8":{},"q9":{},"q10":{},"q11":{},"q12":{},"q13":{},"q14":{},"q15":{},"q16":{},"q17":{},"q18":{},"q19":{},"q20":{},"q21":{},"q22":{},"q23":{},"q24":{},"q25":{},"q26":{},"q27":{},"q28":{},"q29":{},"q30":{},"q31":{},"d0":0.5585262685374610e-308,"d1":0.0000169759663317e-308,"d2":7.9499288951273454e-275,"d3":0,"d4":0,"d5":0,"d6":0,"d7":0,"d8":0,"d9":0,"d10":0,"d11":0,"d12":0,"d13":0,"d14":0,"d15":0,"d16":-1.0855813867524649e+251,"d17":0.000002084565455e-308,"d18":3.0554698911305689e-152,"d19":0.000001322084152e-308,"d20":3.7696966457559071e-175,"d21":1.0004894955531618e+128,"d22":9.5944260775693913e+225,"d23":-6.8154492514793903e-236,"d24":-2.1930923744387854e+50,"d25":-3.1153191556287330e+141,"d26":-1.0031492505499153e+302,"d27":-3.1584786060327890e-284,"d28":-1.7036104465458726e+239,"d29":-1.1538744009003510e-197,"d30":2.4944484440662111e-259,"d31":0,"s0":1.1035169354619356e-39,"s1":1.1210387714598533e-44,"s2":3.8204714345426298e-37,"s3":0,"s4":0,"s5":0,"s6":0,"s7":0,"s8":0,"s9":0,"s10":0,"s11":0,"s12":0,"s13":0,"s14":0,"s15":0,"s16":1.5548730664783592e-21,"s17":-1.3084408235578860e+36,"s18":-8.6088756618229034e-23,"s19":-5.408784745233076e-20,"s20":4.2151578028428229e+37,"s21":18362722504671230,"s22":3.372155249992253e+28,"s23":-7.6784630118876258e-30,"s24":-3715189.5,"s25":2.6426119211413095e+23,"s26":-3.5724724140742379e-23,"s27":-1.6239861190373424e-18,"s28":-205533200,"s29":-0.008124308660626411,"s30":-5.6424941355004989e-36,"s31":0}
x9Value1=0x3050500, x9Value2=0x3050500
```

输出了我们所希望的值：

* `x9`=`0x3050500`

即可实现特定的调试目的。
