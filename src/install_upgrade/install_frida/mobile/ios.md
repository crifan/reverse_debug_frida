# iOS

此处介绍，移动端的iOS（iPhone）中，如何安装Frida（的server）：

* 前提
  * iPhone已越狱
* 安装步骤
  * 用`Sileo`/`Cydia`，添加软件源：`https://build.frida.re`，搜索并安装`frida`，即可
    * Sileo
      * ![sileo_repo_src_frida](../../../assets/img/sileo_repo_src_frida.png)
    * Cydia
      * ![cydia_add_repo_frida](../../../assets/img/cydia_add_repo_frida.png)

## 安装后

### 确保服务端正在正常运行

以及确保iPhone端的`frida-server`的确正在运行，正常运行中：

```bash
iPhone8-150:~ root# ps -A | grep frida
2150 ??         0:00.02 /usr/sbin/frida-server
2194 ttys000    0:00.00 grep frida
```

### frida的位置和版本

安装frida后，可以查看iPhone中的frida的位置和版本：

```bash
iPhone8-150:~ root# frida-server --version
16.0.10

iPhone8-150:~ root# which frida-server
/usr/sbin/frida-server
iPhone8-150:~ root# ls /usr/sbin/frida-server
/usr/sbin/frida-server*
```

### frida插件详情

* frida插件
  * 详情页
    * Sileo
      * ![frida_tweak_detail](../../../assets/img/frida_tweak_detail.png)
    * Cydia
      * ![cydia_frida_details](../../../assets/img/cydia_frida_details.jpg)
  * 已安装的文件
    * 列表
      * `/Library/LaunchDaemons/re.frida.server.plist`
      * `/usr/lib/frida/frida-agent.dylib`
      * `/usr/sbin/frida-server`
    * 图：
      * Sileo
        * ![frida_installed_files](../../../assets/img/frida_installed_files.png)
      * Cydia
        * ![cydia_frida_files](../../../assets/img/cydia_frida_files.jpg)
