# Frida中ObjC的变量类型

## `ObjC`的`Object`

此处的第一个参数和后续的真正的函数的（第一个以及后续的其他）参数，往往是个（iOS中ObjC的）`Class`或`Instance`，在`Frida`中对应的叫做：

* `ObjC`的`Object`

所以常见的操作之一是：

* 转换为ObjC的类
  ```js
    const argSelfObj = new ObjC.Object(argSelf);

    const args2Obj = new ObjC.Object(args2);
    // const args2Obj = new ObjC.Object(ptr(args2));
  ```

以及紧接着的：

* 打印类的信息
  ```js
    console.log("argSelfObj: ", argSelfObj);

    console.log("args2Obj: ", args2Obj);
  ```
  * 典型log输出
    ```bash
    argSelfObj:  <NSXPCConnection: 0x105744c00> connection from pid 46847 on mach service named com.apple.ak.auth.xpc

    args2Obj:  <AKAppleIDAuthenticationService: 0x102d3e4d0>
    ```
* 以及，把`Object`转换成String字符串，再去打印
  ```js
    const args2ObjStr = args2Obj.toString();
    console.log("args2ObjStr: ", args2ObjStr);
  ```
  * 典型log输出
    ```bash
    args2ObjStr:  <AKAppleIDAuthenticationService: 0x102d3e4d0>
    ```
    * 注：此处从调试输出类的信息的角度来说，往往`Object`转换成`String`后再输出的字符串，和上述直接打印`Object`的效果是一样的

### `ObjC`的`Object`的**特殊属性**

对于Frida中的ObjC的Object，有很多内置的特殊的属性：

* `$kind`
* `$super`
* `$superClass`
* `$class`
* `$className`
* `$moduleName`
* `$protocols`
* `$methods`
* `$ivars`

所以，常见操作之一是：

* 查看（当前Object对象的）`类名`=`类的名字`
  ```js
    const argSelfClassName = argSelfObj.$className;
    console.log("argSelfClassName: ", argSelfClassName);

    const args2ObjClassName = args2Obj.$className;
    console.log("args2ObjClassName: ", args2ObjClassName);
  ```
  * 典型log输出
    ```bash
        argSelfClassName:  NSXPCConnection
        args2ObjClassName:  AKAppleIDAuthenticationService
    ```

### `ObjC`的`Object`的**自己类的属性**

而对于本身`ObjC`的类的属性，也是访问的：

* 访问`ObjC`的自己类的属性的方式：把属性当做函数访问 -》加上括号`()`
  * 语法：`curObj.propertyName()`
  * 举例
    * 获取[NSXPCConnection的serviceName](https://developer.apple.com/documentation/foundation/nsxpcconnection/1413751-servicename)
      * 对于Objc的Object对象：`argSelfObj:  <NSXPCConnection: 0x105744c00> connection from pid 46847 on mach service named com.apple.ak.auth.xpc`
        * 去获取其serviceName属性的代码：
          ```js
            const connnServiceNameNSStr = argSelfObj.serviceName();
            console.log("connnServiceNameNSStr: ", connnServiceNameNSStr);
          ```
        * 输出log
          ```bash
          connnServiceNameNSStr:  com.apple.ak.auth.xpc
          ```
    * 获取[NSString的UTF8String](https://developer.apple.com/documentation/foundation/nsstring/1411189-utf8string?language=objc)
      * 代码：
        ```js
          const connnServiceNameJsStr = connnServiceNameNSStr.UTF8String();
          console.log("connnServiceNameJsStr: ", connnServiceNameJsStr);
        ```
        * 输出log
          ```bash
          connnServiceNameJsStr:  com.apple.ak.auth.xpc
          ```

### `ObjC`的`Object`的**自己类的函数**

Frida中，是可以调用Objc中类的函数的。

#### 举例：`+[NSString stringWithString:]`

`ObjC`中`NSString`的原始函数：

```objc
+[NSString stringWithString:@"Hello World"]
```

根据规则：

* 把`冒号`=`:`变成`下划线`=`_`
* 给函数加上括号`()`
* 把参数放到括号`()`中

而变成：

```js
const { NSString } = ObjC.classes;
NSString.stringWithString_("Hello World");
```
  * 或另外一种写法：
    ```js
    ObjC.classes.NSString.stringWithString_("Hello World");
    ```

#### 举例：`-[NSString cStringUsingEncoding:]`

* ObjC的[函数](https://developer.apple.com/documentation/foundation/nsstring/1408489-cstringusingencoding?language=objc)：`-[NSString cStringUsingEncoding:]`
  * -> Frida中的调用：
    ```js
    const NSUTF8StringEncoding = 4;
    const curJsStr = curNsStr.cStringUsingEncoding_(NSUTF8StringEncoding)`
    ```

## `ObjC`的`selector`

`ObjC`中，有个稍微特殊一点的变量，叫做`SEL`=`selector`

也就是前面获取到的，第二个参数，类型是`SEL`

* `SEL`的常见操作之一就是：转换成字符串，再去打印
  ```js
    const argSelStr = ObjC.selectorAsString(argSel);
    console.log("argSelStr: ", argSelStr);
  ```
  * 典型log输出
    ```bash
        argSelStr:  setExportedObject:
    ```

## `ObjC`的`Protocol`

## `ObjC`的`Block`


