# 通用js函数

此处整理Frida的js中，通用的js方面的函数：

* 常用写法：
  * 判断key是否在字典dict中
  	```js
  	curKey in someDict
  	```

## 对象=Object: Dict/List/...

### `toJsonStr`：转换成JSON字符串（用于打印js对象）

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

## String字符串

### `occurrences`：字符串搜索

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

### String - Log日志

```js
// Generate single line log string
// input: logStr="Called: -[NSURLRequest initWithURL:]"
// output: "=============================== Called: -[NSURLRequest initWithURL:] ==============================="
// function generateLineStr(logStr, isWithSpace=true, paddingChar="=", lineWidth=80){
function generateLineStr(logStr, isWithSpace=true, paddingChar="=", lineWidth=100){
// function generateLineStr(logStr, isWithSpace=true, paddingChar="=", lineWidth=120){
	// console.log("logStr=" + logStr, ", isWithSpace=" + isWithSpace + ", paddingChar=" + paddingChar + ", lineWidth=" + lineWidth)
	var lineStr = ""

	var realLogStr = ""
	if (isWithSpace) {
		realLogStr = " " + logStr + " "
	} else {
		realLogStr = logStr
	}

	var realLogStrLen = realLogStr.length
	if ((realLogStrLen % 2) > 0){
		realLogStr += " "
		realLogStrLen = realLogStr.length
	}

	var leftRightPaddingStr = ""
	var paddingLen = lineWidth - realLogStrLen
	if (paddingLen > 0) {
		var leftRightPaddingLen = paddingLen / 2
		leftRightPaddingStr = times(paddingChar, leftRightPaddingLen)
	}

	lineStr = leftRightPaddingStr + realLogStr + leftRightPaddingStr

	// console.log("lineStr:\n" + lineStr)
	return lineStr
}
```

用法举例：

```js
var logStr="Called: -[NSURLRequest initWithURL:]"
generateLineStr(logStr)
```

输出：

```bash
=============================== Called: -[NSURLRequest initWithURL:] ===============================
```

其他输出举例：

```bash
=========== Called: +[WARegistrationURLBuilder preChatdABPropURLWithPhoneNumber:abHash:] ===========

============================= Called: +[NSURLRequest requestWithURL:]  =============================
```

### times：字符串相乘

代码：

```js
// String multiple
// eg: str="=", num=5 => "====="
function times(str, num){
	return new Array(num + 1).join(str)
}
```

用法举例：

```js
var paddingChar = "-"
var leftRightPaddingLen = 10
var leftRightPaddingStr = times(paddingChar, leftRightPaddingLen)
console.log(leftRightPaddingStr)
```

输出：

```bash
----------
```

## List列表

### isItemInList：判断元素是否在列表中

```js
// check whether is item inside the list
// eg: curItem="abc", curList=["abc", "def"] => true
function isItemInList(curItem, curList){
	// method1:
	return curList.includes(curItem)
	// // method2:
	// return curList.indexOf(curItem) > -1
}
```

用法举例：

```js
var iOSObjCallStr = "+[NSURLRequest requestWithURL:]"
let cfgPrintOnceStackExceptionList = [
	"+[NSURLRequest requestWithURL:]",
]

console.log(isItemInList(iOSObjCallStr, cfgPrintOnceStackExceptionList)))
```

输出：

```bash
true
```

