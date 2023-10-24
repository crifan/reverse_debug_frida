# Frida的js通用函数

## isValidPointer：判断指针是否有效

* 背景：
  * 正常的指针值：
    * `0x194d20320`
    * `0x103e79420`
    * `0x2831ac880`
  * 异常的一些指针指：
    * 0x0
    * 0xc

代码：

```js
// check pointer is valid or not
// example
// 		0x103e79560 => true
// 		0xc => false
function isValidPointer(curPtr){
	let MinValidPointer = 0x10000
	var isValid = curPtr > MinValidPointer
	return isValid
}
```

用法举例：

```js
console.log(isValidPointer(0xc))
```

输出：

```bash
false
```
