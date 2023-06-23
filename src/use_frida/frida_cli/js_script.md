# 写js脚本

`frida`的利用方式，从使用角度来说，主要分2类：

* [frida-trace](../../use_frida/frida_trace/README.md)：无需js脚本，直接去hook对应的函数
* `frida ... -l xxx.js`：最常见的用法，通过`js`脚本实现自己的功能
  * 即，写`js`脚本，再用[frida](../../use_frida/frida_cli/README.md)去加载调用

## js脚本文件的类型 + 利用js脚本的方式

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
  * 比如 [iOS的ObjC](../../frida_example/frida/ios_objc/README.md) 中的各种例子
* 也可以，借用别人已有的
  * 比如 [Frida Codeshare](../../frida_overview/doc_refer/frida_codeshare.md)

## js脚本中的内容

主要取决于你的具体使用方法

根据使用场景分，主要有3种：

* `Interceptor`=拦截器
  * 作用：去实现hook函数，打印参数等调试操作
  * 效果：每次匹配到（对应函数），都会触发执行js代码
  * 举例：[Interceptor=hook函数](../../frida_example/frida/ios_objc/hook_func/README.md)
* `ApiResolver`=解析器
  * 效果：只运行一次
    * 只有第一次（解析时）匹配到，才会执行，后续代码运行期间，不再去（匹配）执行
  * 作用
    * > 如果有些情况需要批量 Hook，比如对某一个类的所有方法进行 Hook，或者对某个模块的特定名称的函数进行 Hook，可能我们并不知道准确的名称是什么，只知道大概的关键字，这种情况怎么 Hook 呢？这时就得使用 API查找器（ApiResolver）先把感兴趣函数给找出来，得到地址之后就能 Hook了
  * 举例：[ApiResolver](../../frida_example/frida/ios_objc/apiresolver/README.md)
* `Stalker` = 跟踪器
  * 效果：跟踪代码的实际运行的过程
    * 期间可以打印和查看对应的值，便于实现调试真正代码运行的逻辑
    * 详见：[Frida的Stalker](../../use_frida/frida_cli/frida_stalker.md)
  * 举例：[Stalker](../../frida_example/frida/ios_objc/stalker/README.md)
