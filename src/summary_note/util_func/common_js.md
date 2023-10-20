# 通用js函数

此处整理Frida的js中，通用的js方面的函数：

## `toJsonStr`：转换成JSON字符串（用于打印js对象）

```js
// convert Object(dict/list/...) to JSON string
function toJsonStr(curObj, singleLine=false, space=2){
  // console.log("toJsonStr: singleLine=" + singleLine)
  // var jsonStr = JSON.stringify(curObj, null, 2)
  var jsonStr = JSON.stringify(curObj, null, space)
  if(singleLine) {
    // jsonStr = jsonStr.replace(/\\n/g, '')
    jsonStr = jsonStr.replace(/\n/g, '')
  }
  return jsonStr
  // return curObj.toString()
}
```

用法举例：

```js
console.log("Add: address=" + address + ", moduleInfoDict=" + toJsonStr(moduleInfoDict) + " into cache gAddrToModuleInfoDict")
```

输出：

```bash
[*] Add: address=0x1098ad910, moduleInfoDict={
  "fileName": "/private/var/containers/Bundle/Application/02BED373-87F5-4B48-9F4B-70E853B20267/WhatsApp.app/Frameworks/SharedModules.framework/SharedModules",
  "fileAddress": "0x108c84000",
  "symbolName": "WAGetWCIHttpImpl",
  "symbolAddress": "0x1098abad0"
} into cache gAddrToModuleInfoDict
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
