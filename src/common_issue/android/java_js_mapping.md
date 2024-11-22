# Frida中的Java和js变量类型的映射关系

此处Java中的变量类型，转换成Frida中的类型的写法是：

Frida（的js）中的变量类型，其底层来自于**Dex**

所以其实就是：

* Frida的类型 == Dex中的类型 的语法

即：

* Java变量类型 -》 Frida=Dex变量类型
  * 非数组列表
    * 简单类型
      * 完整写法
        * `int`  `int`
        * `byte`  `byte`
        * `short`  `short`
        * `long`  `long`
        * `float`  `float`
        * `double`  `double`
        * `char`  `char`
      * -》 缩写 ShortyDescriptor
        * 缩写 说明
        * `V`  void；仅对返回类型有效
        * `Z`  boolean
        * `B`  byte
        * `S`  short
        * `C`  char
        * `I`  int
        * `J`  long
        * `F`  float
        * `D`  double
    * 特殊的对象Object == 带父类，带路径的Java的类：前缀L + 点. 变成 斜杠/ + 后缀分号;
      * 语法：`Lfully/qualified/Name;`
      * 说明：类`fully.qualified.Name`
      * 举例
        * `Ljava/lang/String;`
        * `Ljava/lang/Object;`
        * `Ljava/lang/System;`
        * `Ljava/io/PrintStream;`
        * `Ljava/net/InetAddress;`
        * `Ljava/time/LocalDateTime;`
        * `Ljava/lang/ClassLoader;`
  * 列表=数组：前面再加个：左中括号`[`
    * 语法
      * `[descriptor`
    * 说明
      * descriptor 的数组，可递归地用于“数组的数组”，但维数不能超过 255
    * 举例
      * 普通类型：字母缩写
        * `int[]` -> `[I`
        * `byte[]` -> `[B`
        * `short[]` -> `[S`
        * `long[]` -> `[J`
        * `float[]` -> `[F`
        * `double[]` -> `[D`
        * `char[]` -> `[C`
      * 特殊类型：用（Frida=Dex中的）完整类名写法
        * `[Ljava/lang/String;`
        * `[Ljava/lang/Object;`
        * `[Ldalvik/system/DexPathList$Element;`

含义 = 使用场景举例：

（1）Java相关：函数的签名=定义

举例1：

[runtime/native/dalvik_system_DexFile.cc - platform/art - Git at Google (googlesource.com)](https://android.googlesource.com/platform/art/+/refs/heads/master/runtime/native/dalvik_system_DexFile.cc)

```js
  NATIVE_METHOD(DexFile, createCookieWithArray, "([BII)Ljava/lang/Object;"),
```

-》

* `([BII)Ljava/lang/Object;`
  * `([BII)`
    * 括号内是函数参数
      * `[BII`
        * `[B` = `byte[]` = 字节码数组列表
        * `I` = `int`
        * `I` = `int`
  * `Ljava/lang/Object;`
    * 类型：`Object`

-》所以函数的完整定义 类型 就是这样的：

```java
Object createCookieWithArray(byte[], int, int)
```

举例2：

```js
 NATIVE_METHOD(DexFile, isProfileGuidedCompilerFilter, "(Ljava/lang/String;)Z"),
```

-》

* `(Ljava/lang/String;)Z`
  * `(Ljava/lang/String;)`
    * `Ljava/lang/String;`
  * `Z`

-》

```java
void isProfileGuidedCompilerFilter(String)
```

（2）Frida中

Frida中，定义Object数组的写法：

```js
    var JavaObjArr = Java.use("[Ljava.lang.Object;")
    console.log("JavaObjArr=" + JavaObjArr)
```

输出：

```bash
JavaObjArr=<class: [Ljava.lang.Object;>
```

把变量转换为Object的数组：

```js
    var objArr = Java.cast(obj, JavaObjArr)
    console.log("objArr=" + objArr)
```

输出：

```bash
objArr=[Ljava.lang.Object;@fc9e9af
```
