# Frida中ObjC的参数

* 当触发到`Interceptor.attach`的`onEnter`函数后
  * 举例：相关的典型的代码
    ```js
    function hook_specific_method_of_class(className, funcName)
    {
      var curClass = ObjC.classes[className];
      if (typeof(curClass) !== 'undefined') {
        var curMethod = curClass[funcName];
        if (typeof(curMethod) !== 'undefined') {
          Interceptor.attach(curMethod.implementation, {
            onEnter: function(args) {
      ...
    ```
* 传入Frida中`iOS`的`ObjC`的参数 == ObjC的底层函数`objc_msgSend`的参数
  * 注：`objc_msgSend`函数的定义
    ```c
    id objc_msgSend(id self, SEL _cmd, ...)
    ```
* 此时默认参数名叫做`args`的参数 == 是一个参数的**数组**列表，每个元素的类型是`NativePointer`
* 此时对应（Frida中`iOS`的`ObjC`）的参数逻辑是：
  * 第一个参数 = `args[0]` == `objc_msgSend`的`self`(类型是`id`)
  * 第二个参数 = `args[1]` == `objc_msgSend`的`_cmd`（类型是`SEL`）
  * 后续才是：真正的函数的参数
    * 函数的真正的第1个参数 = `args[2]`
    * 函数的真正的第2个参数 = `args[3]`
    * ...

## Frida中获取ObjC的参数

### 第一个参数`args[0]`

* 获取第一个参数`args[0]`
  ```js
    // args[0] is self = id
    const argSelf = args[0];
    console.log("argSelf: ", argSelf);
  ```
  * 典型log输出
    ```bash
    argSelf:  0x105744c00
    ```

### 第二个参数`args[1]`

获取第二个参数`args[1]`：

```js
  // args[1] is selector
  const argSel = args[1];
  console.log("argSel: ", argSel);
```
  * 典型log输出
    ```bash
    argSelf:  0x105744c00
    ```

### ObjC函数的真正参数

* 获取 函数的真正的第1个参数 = `args[2]`
  ```js
    // args[2] holds the first function argument
    const args2 = args[2];
    console.log("args2: ", args2);
  ```
  * 典型log输出
    ```bash
    args2:  0x105743850
    ```
* 获取 函数的其他更多参数（如果有的话）
  ```js
    const args3 = args[3];
    console.log("args3: ", args3);

    const args4 = args[4];
    console.log("args4: ", args4);
    ...
  ```

#### 计算ObjC的函数的真正参数的个数 + 打印全部参数

---

TODO：把这部分代码，整理成 独立的工具类函数，转移到：[工具类函数](../../../summary_note/util_func.md)

---

根据ObjC的基础知识，目前得知：可以通过`SEL`=`selector`字符串中的`:`=`冒号`的个数，判断后续真正有几个参数

所以，可以用如下代码，循环的、批量的、挨个、打印所有参数：

```js
/** Function that count occurrences of a substring in a string;
 * @param {String} string               The string
 * @param {String} subString            The sub string to search for
 * @param {Boolean} [allowOverlapping]  Optional. (Default:false)
 *
 * @author Vitim.us https://gist.github.com/victornpb/7736865
 * @see Unit Test https://jsfiddle.net/Victornpb/5axuh96u/
 * @see https://stackoverflow.com/a/7924240/938822
 */
function occurrences(string, subString, allowOverlapping) {
	// console.log("string=" + string + ",subString=" + subString + ", allowOverlapping=" + allowOverlapping)
	string += "";
	subString += "";
	if (subString.length <= 0) return (string.length + 1);

	var n = 0,
		pos = 0,
		step = allowOverlapping ? 1 : subString.length;

	while (true) {
		pos = string.indexOf(subString, pos);
		// console.log("pos=" + pos)
		if (pos >= 0) {
			++n;
			pos += step;
		} else break;
	}

	return n;
}

...

function hook_specific_method_of_class(className, funcName)
{
		var iOSObjCallStr = toiOSObjcCall(className, funcName)
    // var hook = ObjC.classes[className][funcName];
    // var hook = eval('ObjC.classes.' + className + '["' + funcName + '"]');
		var curClass = ObjC.classes[className];
		if (typeof(curClass) !== 'undefined') {
			var curMethod = curClass[funcName];
			if (typeof(curMethod) !== 'undefined') {
				Interceptor.attach(curMethod.implementation, {
					onEnter: function(args) {
						console.log("=========== [*] Detected call to: " + iOSObjCallStr);
            ...

            const argSelStr = ObjC.selectorAsString(argSel);
            console.log("argSelStr: ", argSelStr);
            const argCount = occurrences(argSelStr, ":");

            // console.log("funcName=", funcName);
            // const argCount = occurrences(funcName, ":");
            // console.log("argCount: ", argCount);

            for (let curArgIdx = 0; curArgIdx < argCount; curArgIdx++) {
              const curArg = args[curArgIdx + 2];

              // const usePtr = false;
              const usePtr = true;
              if (usePtr) {
                // console.log("usePtr=", usePtr);
                const curArgPtr = ptr(curArg);
                console.log("---------- [" + curArgIdx + "] curArgPtr=" + curArgPtr);
                if (!curArgPtr.isNull()) {
                  const curArgPtrObj = new ObjC.Object(curArgPtr);
                  console.log("curArgPtrObj: ", curArgPtrObj);
                  console.log("curArgPtrObj className: ", curArgPtrObj.$className);
                }
              } else {
                console.log("---------- [" + curArgIdx + "] curArg=" + curArg);
                if (curArg && (curArg != 0x0)) {
                  // console.log("curArg className: ", curArg.$className);
                  const curArgObj = new ObjC.Object(curArg);
                  console.log("curArgObj: ", curArgObj);
                  console.log("curArgObj className: ", curArgObj.$className);
                }
              }
            }

            ...
```

某次的输出日志的效果：

```bash
=========== [*] Detected call to: +[NSURLRequest requestWithURL:cachePolicy:timeoutInterval:]
funcName= + requestWithURL:cachePolicy:timeoutInterval:
argSelStr:  requestWithURL:cachePolicy:timeoutInterval:
argCount:  3
---------- [0] curArgPtr=0x2831f2530
curArgPtrObj:  https://setup.icloud.com/setup/signin/v2/login
curArgPtrObj className:  NSURL
---------- [1] curArgPtr=0x0
---------- [2] curArgPtr=0x0
```

* 注意：
  * 当Frida调试期间，输出log日志太多，或者打印参数太多时，经常会导致app或进程崩溃而无法继续调试
    * 所以当遇到Frida调试导致的崩溃问题时，可以适当的减少log日志的输出，以缓解崩溃问题
