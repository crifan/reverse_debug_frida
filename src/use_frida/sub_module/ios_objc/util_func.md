# 工具类函数

Frida调试iOS的ObjC期间，整理了些，工具类的函数，供参考。

## `nsStrToJsStr`：NSString转换为js的String

代码：

```js
// NSString instance -> js string
function nsStrToJsStr(curNsStr){
  // method 1
  return curNsStr.UTF8String()
  // // method 2:
  // const NSUTF8StringEncoding = 4;
  // https://developer.apple.com/documentation/foundation/nsstring/1408489-cstringusingencoding?language=objc
  // return curNsStr.cStringUsingEncoding_(NSUTF8StringEncoding)
}
```

用法举例：

```js
          const connnServiceNameNSStr = nsxpcconnectionObj.serviceName();
          const connnServiceNameJsStr = nsStrToJsStr(connnServiceNameNSStr)
          console.log("connnServiceNameNSStr: ", connnServiceNameNSStr);
          console.log("connnServiceNameJsStr: ", connnServiceNameJsStr);
```

输出：

```bash
connnServiceNameNSStr:  com.apple.ak.auth.xpc
connnServiceNameJsStr:  com.apple.ak.auth.xpc
```

## `occurrences`：字符串搜索

```js
/** Function that count occurrences of a substring in a string;
 * @param {String} string               The string
 * @param {String} subString            The sub string to search for
 * @param {Boolean} [allowOverlapping]  Optional. (Default:false)
 *
 * @author Vitim.us https://gist.github.com/victornpb/7736865
 * @see Unit Test https://jsfiddle.net/Victornpb/5axuh96u/
 * @see https://stackoverflow.com/a/7924240/938822
 * @example occurrences("- setExportedObject:", ":") => 1
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
```

用法举例：

```js
            const argSel = args[1];
            console.log("argSel: ", argSel);
            const argSelStr = ObjC.selectorAsString(argSel);
            console.log("argSelStr: ", argSelStr);

            const argCount = occurrences(argSelStr, ":");
            console.log("argCount: ", argCount);
```

输出：

```bash
argSel:  0x19cf48d27
argSelStr:  fileURLWithPath:isDirectory:
argCount:  2
```

含义：

（iOS的ObjC中的SEL）字符串：`fileURLWithPath:isDirectory:` 中有2个冒号`:`

## `toiOSObjcCall`

```js
// convert from frida function call to ObjC function call
// "NSURL", "- initWithString:" => "-[NSURL initWithString:]"
function toiOSObjcCall(class_name, method_name){
    const instanceCallStart = "-[" + class_name + " ";
    const classCallStart = "+[" + class_name + " ";
    var objcFuncCall = method_name.replace("- ", instanceCallStart);
    objcFuncCall = objcFuncCall.replace("+ ", classCallStart);
    objcFuncCall = objcFuncCall + "]";
    // console.log(class_name + " -> " + method_name + " => " + objcFuncCall);
    return objcFuncCall;
}
```

用法举例：

```js
function hook_specific_method_of_class(className, funcName)
{
  console.log("className=" + className + ", funcName=" + funcName)
  var iOSObjCallStr = toiOSObjcCall(className, funcName)
  console.log("iOSObjCallStr=", iOSObjCallStr)
  ...
```

输出：

```bash
className=NSXPCConnection, funcName=- setExportedObject:
iOSObjCallStr= -[NSXPCConnection setExportedObject:]
```

## `toFridaObjcCall`

```js
// convert from ObjC function call to frida function call
// "-[NSURL initWithString:]" => ["NSURL", "- initWithString:"]
function toFridaObjcCall(fridaObjcCallStr){
    // console.log("fridaObjcCallStr=" + fridaObjcCallStr)
    const funcCallP = /^([+-])\[(\w+)\s+([\w:]+)\]$/
    // console.log("funcCallP=" + funcCallP)
    const funcCallMatch = fridaObjcCallStr.match(funcCallP)
    // console.log("funcCallMatch=" + funcCallMatch)
    const objcFuncTypeChar = funcCallMatch[1]
    const objcClassName = funcCallMatch[2]
    const objcFuncName = funcCallMatch[3]
    const fridaFuncCallList = [objcClassName, objcFuncTypeChar + " " + objcFuncName]
    // console.log("fridaFuncCallList=" + fridaFuncCallList)
    return fridaFuncCallList
}
```

用法举例：

* 输入
  * `"+[NSURL URLWithString:]"`
  * `"-[NSURLConnection start]"`
  * `"-[NSURL initWithString:relativeToURL:]"`
* 输出
  * `["NSURL", "+ URLWithString:"]`
  * `["NSURLConnection", "- start"]`
  * `["NSURL", "- initWithString:relativeToURL:"]`
