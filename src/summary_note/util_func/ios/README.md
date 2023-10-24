# iOS函数

此处整理Frida的js中，iOS（的OjbC）方面的函数：

## `objcObjToStr`：ObjC的Object转换为C的string

```js
// convert ObjC Object to C string
function objcObjToStr(objcObj){
  return objcObj.toString()
}
```

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

## `toiOSObjcCall`：把Firda的ObjC函数写法，转换成ObjC的函数写法

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

其他举例：

* `"NSURL", "- _cfurl"` => `"-[NSURL _cfurl]"`
* `"NSURL"`, `"+ fileURLWithPath:isDirectory:"` => `+[NSURL fileURLWithPath:isDirectory:]`
* `"NSURL"`, `"- initFileURLWithPath:isDirectory:"` => `"-[NSURL initFileURLWithPath:isDirectory:]"`

## `toFridaObjcCall`：把ObjC函数写法，转换成Frida的ObjC函数写法

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

## `initCommonLibFunctions`：初始化常用库中的常用函数

代码：

```js
/*******************************************************************************
 * Global Variables
*******************************************************************************/

var free = null
var objc_getClass = null
var class_copyMethodList = null
var objc_getMetaClass = null
var method_getName = null
var dladdr = null
var _dyld_image_count = null
var _dyld_get_image_name = null
var _dyld_get_image_vmaddr_slide = null
var objc_copyClassNamesForImage = null


/******************** iOS Common Lib Functions ********************/

function initCommonLibFunctions(){
	console.log("Init common functions in common libs:")

// free
	free = new NativeFunction(
		Module.findExportByName(null, 'free'),
		'void',
		['pointer']
	)
	console.log("free=" + free)

	objc_getClass = new NativeFunction(
		Module.findExportByName(null, 'objc_getClass'),
		'pointer',
		['pointer']
	)
	console.log("objc_getClass=" + objc_getClass)

	class_copyMethodList = new NativeFunction(
		Module.findExportByName(null, 'class_copyMethodList'),
		'pointer',
		['pointer', 'pointer']
	)
	console.log("class_copyMethodList=" + class_copyMethodList)

	objc_getMetaClass = new NativeFunction(
		Module.findExportByName(null, 'objc_getMetaClass'),
		'pointer',
		['pointer']
	)
	console.log("objc_getMetaClass=" + objc_getMetaClass)

	method_getName = new NativeFunction(
		Module.findExportByName(null, 'method_getName'),
		'pointer',
		['pointer']
	)
	console.log("method_getName=" + method_getName)

	/*
	int dladdr(const void *, Dl_info *);

	typedef struct dl_info {
					const char      *dli_fname;     // Pathname of shared object
					void            *dli_fbase;     // Base address of shared object
					const char      *dli_sname;     // Name of nearest symbol
					void            *dli_saddr;     // Address of nearest symbol
	} Dl_info;

	*/
	dladdr = new NativeFunction(
		Module.findExportByName(null, 'dladdr'),
		'int',
		['pointer','pointer']
	)
	console.log("dladdr=" + dladdr)

	// uint32_t  _dyld_image_count(void)
	_dyld_image_count = new NativeFunction(
		Module.findExportByName(null, '_dyld_image_count'),
		'uint32',
		[]
	)
	console.log("_dyld_image_count=" + _dyld_image_count)

	// const char*  _dyld_get_image_name(uint32_t image_index) 
	_dyld_get_image_name = new NativeFunction(
		Module.findExportByName(null, '_dyld_get_image_name'),
		'pointer',
		['uint32']
	)
	console.log("_dyld_get_image_name=" + _dyld_get_image_name)


	// intptr_t   _dyld_get_image_vmaddr_slide(uint32_t image_index)
	_dyld_get_image_vmaddr_slide = new NativeFunction(
		Module.findExportByName(null, '_dyld_get_image_vmaddr_slide'),
		'pointer',
		['uint32']
	)
	console.log("_dyld_get_image_vmaddr_slide=" + _dyld_get_image_vmaddr_slide)

	// const char * objc_copyClassNamesForImage(const char *image, unsigned int *outCount)
	objc_copyClassNamesForImage = new NativeFunction(
		Module.findExportByName(null, 'objc_copyClassNamesForImage'),
		'pointer',
		['pointer', 'pointer']
	);
	console.log("objc_copyClassNamesForImage=" + objc_copyClassNamesForImage)
}
```

用法举例：

```js
/*******************************************************************************
 * Main
*******************************************************************************/
initCommonLibFunctions()
```

输出：

```bash
Init common functions in common libs:
free=0x190f57f10
objc_getClass=0x190f9c098
class_copyMethodList=0x190f96cf8
objc_getMetaClass=0x190f9c100
method_getName=0x190f93b54
dladdr=0x191068174
_dyld_image_count=0x191067b40
_dyld_get_image_name=0x19106806c
_dyld_get_image_vmaddr_slide=0x191068004
objc_copyClassNamesForImage=0x190f97e68
```

说明：

后续其他地方，即可调用这些函数：

```js
	free(pClasses)
...

function get_info_form_address(address){
...
	var dl_info = Memory.alloc(Process.pointerSize*4);
	dladdr(ptr(address), dl_info)
```
