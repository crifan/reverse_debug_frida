# XinaA15中的frida

* XinaA15
  * 截至`XinaA15 v1.1.8` + `Sileo Nightly v2.4` + `Frida v16.0.11`：在rootless越狱的`XinaA15`中，无法通过`Sileo Nightly`正常安装和使用`Frida`
    * 只要一使用frida工具（比如`frida-ps -U`等）就会导致iPhone重启
    * 且安装和卸载都会出现一些异常报错
      * 安装Frida
        * `/var/jb/var/ib/Library/LaunchDaemons/re.frida.server.plist service is disabled`
        * 且此处`Sileo Nightly v2.4`中看到的最新版`Frida v16.0.13`，竟然还会出现无法安装：404错误
          * ![sileo_frida_install_404](../../assets/img/sileo_frida_install_404.jpg)
      * 卸载Frida
        * `/var/jb/var/jb/Library/LaunchDaemons/re.frida.server.plist: Could not find specified service`
