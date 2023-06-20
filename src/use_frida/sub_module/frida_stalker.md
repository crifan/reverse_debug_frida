# frida的Stalker


# 举例

### 用`frida`的`Stalker`调试`akd`的函数arm64的`___lldb_unnamed_symbol2575$$akd`

frida命令：

```bash
frida -U -n akd -l ./fridaStalker_akdSymbol2575.js
```

* `fridaStalker_akdSymbol2575.js`文件内容：

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
                        //     + ",regsRead=" + JSON.stringify(instruction.regsRead)
                        //     + ",regsWritten=" + JSON.stringify(instruction.regsWritten)
                        //     + ",groups=" + JSON.stringify(instruction.groups)
                        //     + ",toString()=" + instruction.toString()
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
