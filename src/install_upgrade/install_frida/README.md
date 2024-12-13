# 安装Frida

在能使用`Frida`之前，先要去安装`Frida`=初始化Frida调试环境：

* 概述
  * 电脑端安装`frida`（和`frida-tools`），移动端安装`frida`（的**server**）
    * Mac中逆向iOS
      * Mac：
        * 安装`frida`和安装`frida-tools`
          ```bash
          pip3 install frida
          pip3 install frida-tools
          ```
          * 如果要：（1）强制安装到全局版本Python （2）加上代理，则是：
            ```bash
            pip3 --proxy http://127.0.0.1:58591 install frida --break-system-packages
            pip3 --proxy http://127.0.0.1:58591 install frida-tools --break-system-packages
            ```
      * iOS（iPhone）：包管理器（`Sileo`/`Cydia`）中（通过软件源：`https://build.frida.re`）安装`frida`的插件
    * Mac中逆向Android
      * Mac：**同上**
      * Android：`Magisk`中安装[MagiskFrida](https://github.com/ViRb3/magisk-frida)插件
        * 注：Frida官网的Android版`frida-server`有问题，所以换装第三方可用版本

后续详细解释。
