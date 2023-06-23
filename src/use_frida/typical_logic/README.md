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
