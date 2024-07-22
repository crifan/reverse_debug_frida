# 移动端

此处介绍，移动端中如何安装Firda：

* 核心思路
  * 给移动端安装一个frida的server（`frida-server`）
* 举例
  * iOS（iPhone）
    * 包管理器（`Sileo`/`Cydia`）中（通过软件源：`https://build.frida.re`）安装`frida`的插件
  * Android
    * `Magisk`中安装[MagiskFrida](https://github.com/ViRb3/magisk-frida)插件
      * 说明
        * Frida官网的Android版`frida-server`有问题，所以换装第三方可用版本
