# 写js脚本

`frida`的利用方式，从使用角度来说，主要分2类：

* `frida-trace`：无需js脚本，直接去hook对应的函数
* `frida ... -l xxx.js`：最常见的用法，通过`js`脚本实现自己的功能
  * 即，写`js`脚本，再用`frida`去加载调用
  * 注：关于`frida`命令本身的使用，详见[frida](../../use_frida/sub_module/frida.md)

# js脚本文件的类型 + 利用js脚本的方式

* 最早的，大家用的最多的，最常见的：`.js`文件=`JavaScript`文件
  * 对应用法
    ```bash
    frida ... -l xxx.js
    ```
    * 举例
      ```bash
      frida -U -p 8229 -l hookAccountLogin_singleClassSingleMethod.js
      ```
* 最新的，官网推荐的：`.ts`文件=`TypeScript`文件
  * 对应用法
    ```bash
    frida ... -l xxx.ts
    ```

## js脚本的来源

* 可以自己写
* 也可以，借用别人已有的
  * 比如：`Frida Codeshare`
    * 详见：[Frida Codeshare](../../frida_overview/doc_refer/frida_codeshare.md)

## js脚本中的内容

主要取决于你的具体使用方法

根据使用场景分，主要有3种：

* `Interceptor`=拦截器
  * 作用：去实现hook函数，打印参数等调试操作
  * 效果：每次匹配到（对应函数），都会触发执行js代码
* `ApiResolver`=解析器
  * 效果：只运行一次
    * 只有第一次（解析时）匹配到，才会执行，后续代码运行期间，不再去（匹配）执行
  * 作用
    * > 如果有些情况需要批量 Hook，比如对某一个类的所有方法进行 Hook，或者对某个模块的特定名称的函数进行 Hook，可能我们并不知道准确的名称是什么，只知道大概的关键字，这种情况怎么 Hook 呢？这时就得使用 API查找器（ApiResolver）先把感兴趣函数给找出来，得到地址之后就能 Hook了
* `Stalker` = 跟踪器
  * 效果：跟踪代码的实际运行的过程
    * 期间可以打印和查看对应的值，便于实现调试真正代码运行的逻辑
    * 详见：[Frida的Stalker](../../use_frida/sub_module/frida_stalker.md)

### js脚本内容举例

此处给出一些例子，便于了解，不同类型用法，js内部代码大概长什么样：

#### Interceptor

```bash
frida -U -F  -p 18533 -l ./hookAccountLogin_Interceptor.js
```

`hookAccountLogin_Interceptor.js`内容如下：

```js
// https://raw.githubusercontent.com/noobpk/frida-ios-hook/master/frida-ios-hook/frida-scripts/hook-all-methods-of-specific-class.js
function hook_class_method(class_name, method_name)
{
    var hook = ObjC.classes[class_name][method_name];
        Interceptor.attach(hook.implementation, {
            onEnter: function(args) {
            console.log("[*] Detected call to: " + class_name + " -> " + method_name);
        }
    });
}

function run_hook_all_methods_of_specific_class(className_arg)
{
    console.log("[*] Started: Hook all methods of a specific class");
    console.log("[+] Class Name: " + className_arg);
    //Your class name here
    var className = className_arg;
    //var methods = ObjC.classes[className].$methods;
    var methods = ObjC.classes[className].$ownMethods;
    for (var i = 0; i < methods.length; i++)
    {
        console.log("[-] "+methods[i]);
        console.log("\t[*] Hooking into implementation");
        //eval('var className2 = "'+className+'"; var funcName2 = "'+methods[i]+'"; var hook = eval(\'ObjC.classes.\'+className2+\'["\'+funcName2+\'"]\'); Interceptor.attach(hook.implementation, {   onEnter: function(args) {    console.log("[*] Detected call to: " + className2 + " -> " + funcName2);  } });');
        var className2 = className;
        var funcName2 = methods[i];
        hook_class_method(className2, funcName2);
        console.log("\t[*] Hooking successful");
    }
    console.log("[*] Completed: Hook all methods of a specific class");
}

function hook_all_methods_of_specific_class(className_arg)
{
    setImmediate(run_hook_all_methods_of_specific_class,[className_arg])
}

//Your class name goes here
hook_all_methods_of_specific_class("AAUISignInViewController")
```

#### ApiResolver

```bash
frida -U -p 18031 -l ./hookAccountLogin.js
```

`hookAccountLogin.js`内容如下：

```js
var resolver = new ApiResolver('objc');
resolver.enumerateMatches('*[AAUISignInViewController *]', {
  onMatch: function(match) {
    console.log(match['name'] + ":" + match['address']);
  },


  onComplete: function() {}
});
```

#### Stalker

```bash
frida -U -n akd -l ./fridaStalker_akdSymbol2575.js
```

`fridaStalker_akdSymbol2575.js`文件内容：

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
const funcRealStartAddr = moduleBaseAddress.add(funcRelativeStartAddr);

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
                console.log(">>> into onReceive: parsedEvents=" + parsedEvents);
            },
    
            transform: function (iterator) {
                // console.log("iterator=" + iterator);
                var instruction = iterator.next();
                const startAddress = instruction.address;
                const gt_realStartAddr = startAddress.compare(funcRealStartAddr) >= 0
                const lt_realEndAddr = startAddress.compare(funcRealEndAddr) < 0
                var isAppCode = gt_realStartAddr && lt_realEndAddr
                console.log("+++ into iterator: startAddress=" + startAddress + ", isAppCode=" + isAppCode);

                do {
                    if (isAppCode) {
                        // is origal function code = which we focus on

                        var curRealAddr = instruction.address;
                        var curOffsetHexPtr = curRealAddr.sub(funcRealStartAddr)
                        var curOffsetInt = curOffsetHexPtr.toInt32()
      
                        var instructionStr = instruction.toString()
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
