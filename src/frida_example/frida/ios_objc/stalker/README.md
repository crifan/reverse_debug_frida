# Stalker

## 别处某示例

参考自：[【iOS逆向】某营业厅算法分析_小陈_InfoQ写作社区](https://xie.infoq.cn/article/4c1fe5ca87269a7a5ac1645fc)

`ts`=`TypeScript`代码：

```js
var addr = 0x0000000029FD08 // loginWithPhoneNbr函数的起始地址
var mainModule = Process.enumerateModules()[0];
console.log(JSON.stringify(mainModule));
var mainName: string = mainModule.name;
var baseAddr = Module.findBaseAddress(mainName)!;
Interceptor.attach(baseAddr.add(addr), {
    onEnter: function(args) {
        console.log(addr.toString(16), "= loginWithPhoneNbr onEnter =");
        var tid = Process.getCurrentThreadId();
        Stalker.follow(tid, {
            events: {
                call: true, // CALL instructions: yes please            
                ret: false, // RET instructions
                exec: false, // all instructions: not recommended as it's
                block: false, // block executed: coarse execution trace
                compile: false // block compiled: useful for coverage
            },
            transform: (iterator: StalkerArm64Iterator) => {
                let instruction = iterator.next();
                const startAddress = instruction!.address;
                var isAppCode = startAddress.compare(baseAddr.add(addr)) >= 0 && startAddress.compare(baseAddr.add(addr).add(10000)) === -1;
                do {
                    if (isAppCode) {
                        if (instruction!.mnemonic === "bl") {
                            iterator.putCallout((ctx) => {
                                var arm64Context = ctx as Arm64CpuContext;
                                console.log("bl x0 = " + new ObjC.Object(arm64Context.x0))
                                console.log("bl x1 = " + arm64Context.x1.readCString())
                            });                        
                        }
                    }
                    iterator.keep();
                } while ((instruction = iterator.next()) !== null);
             }
        })
    }, onLeave: function(retval) { 
        console.log("retval:", new ObjC.Object(retval))
        console.log(addr.toString(16), "= loginWithPhoneNbr onLeave =");
    }
});
```

## frida-stalker-example

别处的例子：

[misc/frida-stalker-example.py at master · poxyran/misc · GitHub](https://github.com/poxyran/misc/blob/master/frida-stalker-example.py)

```py
import sys
import frida

def on_message(message, data):
    print "[%s] -> %s" % (message, data)

def main(target_process):
    session = frida.attach(target_process)
    script = session.create_script("""
function StalkerExeample() 
{
    var threadIds = [];

    Process.enumerateThreads({
        onMatch: function (thread) 
        {
            threadIds.push(thread.id);
            console.log("Thread ID: " + thread.id.toString());
        },

        onComplete: function () 
        {
            threadIds.forEach(function (threadId) 
                {
                    Stalker.follow(threadId, 
                    {
                        events: {call: true},
                    
                    onReceive: function (events)
                    {
                        console.log("onReceive called.");
                    },
                    onCallSummary: function (summary)
                    {
                        console.log("onCallSummary called.");
                    }
                });
            });
        }
    });
}

StalkerExeample();
""")
    script.on('message', on_message)
    script.load()
    raw_input('[!] Press <Enter> at any time to detach from instrumented program.\n\n')
    session.detach()

if __name__ == '__main__':
    if len(sys.argv) < 2:
        print 'Usage: %s <process name or PID>' % __file__
        sys.exit(1)

    try:
        target_process = int(sys.argv[1])
    except ValueError:
        target_process = sys.argv[1]

    main(target_process)
```
