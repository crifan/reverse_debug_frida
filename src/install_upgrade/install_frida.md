# 安装Frida

在能使用Frida之前，先要去安装Frida：

* PC端
  * Mac
    * 安装`frida` => 使得有`frida`命令行工具可用
      ```bash
      pip3 install frida
      ```
      * 安装后查看版本
        ```bash
        pip show frida
        ```
    * 安装`frida-tools` => 使得有[frida-trace](../use_frida/sub_module/frida_tools/frida_trace.md)、[frida-ps](../use_frida/sub_module/frida_tools/frida_ps.md)、[frida-ls](../use_frida/sub_module/frida_tools/frida_ls.md)等命令行工具可用
      ```bash
      pip3 install frida-tools
      ```
      * 安装后查看版本
        ```bash
        pip show frida-tools
        ```
      * 额外说明
        * 如果前面没有先单独安装`frida`，则安装`frida-tools`时会自动安装所依赖的`frida`
    * 安装Frida的`gadget` => 使得后续使用[frida-ios-dump](https://book.crifan.org/books/ios_re_crack_shell_ipa/website/crack_example/frida_ios_dump/)时而不报错`need Gadget to attach on jailed iOS`
      * 下载gadget库文件
        * 从[Frida的Github的release](https://github.com/frida/frida/releases/)页面中，下载对应版本的`frida-gadget`的`dylib`
          * 举例
            * https://github.com/frida/frida/releases/download/16.0.8/frida-gadget-16.0.8-ios-universal.dylib.gz
              * 解压得到：`frida-gadget-16.0.8-ios-universal.dylib`
      * 拷贝到对应位置：`~/.cache/frida/gadget-ios.dylib`
        * 举例
          ```bash
          cp frida-gadget-16.0.8-ios-universal.dylib /Users/crifan/.cache/frida/gadget-ios.dylib
          ```
* 移动端
  * （越狱）iPhone
    * 安装步骤
      * 用`Sileo`/`Cydia`，添加软件源：`https://build.frida.re`，搜索并安装`frida`，即可
        * Sileo
          * ![sileo_repo_src_frida](../assets/img/sileo_repo_src_frida.png)
        * Cydia
          * ![cydia_add_repo_frida](../assets/img/cydia_add_repo_frida.png)
    * Frida安装后
      * 确保iPhone端`frida-server`已经正常运行
        ```bash
        iPhone8-150:~ root# ps -A | grep frida
        2150 ??         0:00.02 /usr/sbin/frida-server
        2194 ttys000    0:00.00 grep frida
        ```
      * 插件详情页
        * Sileo
          * ![frida_tweak_detail](../assets/img/frida_tweak_detail.png)
        * Cydia
          * ![cydia_frida_details](../assets/img/cydia_frida_details.jpg)
      * 已安装的文件
        * 列表
          * `/Library/LaunchDaemons/re.frida.server.plist`
          * `/usr/lib/frida/frida-agent.dylib`
          * `/usr/sbin/frida-server`
        * 图：
          * Sileo
            * ![frida_installed_files](../assets/img/frida_installed_files.png)
          * Cydia
            * ![cydia_frida_files](../assets/img/cydia_frida_files.jpg)
