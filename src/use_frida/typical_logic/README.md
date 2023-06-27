# Frida典型使用逻辑

* 先搞懂大的概念
  * 从逻辑概念角度分
    * 核心工具：`frida`命令行
    * 辅助工具：`frida-tools`
      * 包括`frida-trace`、`frida-ps`、`frida-ls`等工具
  * 从使用角度分
    * 最常用：`frida`命令行
      * 分析调试逻辑的主力工具
    * 经常用：`frida-trace`
      * 分析函数调用堆栈关系
    * 偶尔用：`frida-tools`（包括`frida-ps`、`frida-ls`等）
      * 其他辅助用途，比如查看进程PID等
* 去hook调试iPhone中的app或二进制 = Frida典型使用逻辑
  * 准备工作
    * 先要搞懂当前连接了哪些iPhone设备：`frida-ls-deviecs`
    * 再去搞懂要去hook的目标（app或二进制）：`frida-ps` （或ssh连接的iPhone中的ps）
    * 辅助：查看iPhone中有哪些相关文件：`frida-ls`
  * 真正去hook调试
    * 前期用=经常用：`frida-trace` -》调试（app页面操作过程中都）调用了哪些ObjC的（类和）函数
    * 后期用=最常用：`frida` -》 写js去调试具体的ObjC的函数
      * 具体干什么
        * 查看
          * 传入的参数的类型和值
          * 函数调用堆栈
        * 微调=改动
          * （临时）修改传入参数的值-》研究程序逻辑变化
          * （临时）修改返回值-》研究程序逻辑变化
      * 常用到的内部功能
        * 主要是：`Interceptor`（的`Interceptor.attach`）-》 去拦截=触发=hook对应的类的函数
        * 偶尔用：`ApiResolver` -》 （模糊）搜索有哪些类，类有哪些函数等
        * 高级用法：`Stalker` -》 研究（被混淆的）代码实际的执行过程和逻辑等

## 用Frida辅助iOS逆向的思路

* iOS逆向开发心得=思路：利用frida，打印函数调用 -》找到url、找到全部被调函数

对于iOS逆向的话，有空试试 frida的hook。

就像之前别的某一个iOS技术人员沟通的方法：**Hook所有方法调用**

比如 估计是 hook `objc_msgSend` 这类函数？

都打印出来 就知道调用了哪些类的哪些函数了，甚至可以加上额外的统计调用次数之类的。

以及也可以把后面的几个参数的值，只要是字符串的也都打印出来。

另外去hook url调用HTTP的。

估计也是能轻易的搞清楚当前调用过哪些网络请求以及相关的什么参数。

估计就能找出来`NSUrlRequet`之类的相关调用和url和其他参数。

总之有空还是去多试试frida的hook。

【后记1】

其实就是`所有类`的`所有方法`

之前试过，效果还行。

不过要排除掉输出太多的，否则容易卡死崩溃。

【后记2】

有机会去试试：

* frida去hook：
  * objc_msgSend函数
    * 看看有多少objc的函数调用
      * 不过突然想到，貌似就是：`frida-trace`加上`-m *[* *]`的意思？

【后记3】
> Hook所有方法调用

看来应该是：

* 用Frida去hook：所有类的所有方法？
* 或者是：frida-trace去hook所有类的所有方法
* 又或者是：只hook objc_msgSend方法？这样也能打印出所有ObjC函数的调用

有空去试试
