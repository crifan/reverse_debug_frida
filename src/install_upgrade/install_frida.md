# 安装Frida

* PC端
  * Mac
    * 安装`frida`
      ```bash
      pip3 install frida
      ```
      * 安装后查看版本
        ```bash
        pip show frida
        ```
    * 安装`frida-tools`
      ```bash
      pip3 install frida-tools
      ```
      * 安装后查看版本
        ```bash
        pip show frida-tools
        ```
* 移动端
  * （越狱）iPhone
    * 用`Sileo`/`Cydia`，添加软件源：`https://build.frida.re`，搜索并安装`frida`，即可
      * Frida安装后的插件详情页
        * ![frida_tweak_detail](../assets/img/frida_tweak_detail.png)
      * Frida安装后的文件
        * 列表
          * `/Library/LaunchDaemons/re.frida.server.plist`
          * `/usr/lib/frida/frida-agent.dylib`
          * `/usr/sbin/frida-server`
        * 图：已安装的文件
          * ![frida_installed_files](../assets/img/frida_installed_files.png)
