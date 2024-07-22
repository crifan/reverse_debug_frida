# Mac

此处介绍，电脑端Mac中，安装Frida的过程：

## 安装

### 必选

#### 安装`frida`

* 安装`frida` => 使得有[frida命令行工具](../../../use_frida/frida_cli/README.md)可用
  ```bash
  pip3 install frida
  ```
  * 安装后查看版本
    ```bash
    pip show frida
    ```

#### 安装`frida-tools` 

* 安装`frida-tools` => 使得有[frida-trace](../../../use_frida/frida_trace/README.md)、[frida-ps](../../../use_frida/frida_tools/frida_ps.md)、[frida-ls](../../../use_frida/frida_tools/frida_ls.md)等命令行工具可用
  ```bash
  pip3 install frida-tools
  ```
  * 安装后查看版本
    ```bash
    pip show frida-tools
    ```
  * 额外说明
    * 如果前面没有先单独安装`frida`，则安装`frida-tools`时会自动安装所依赖的`frida`

### 可选

#### 安装Frida的`gadget`

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

#### 安装：Frida的Node.js的bindings

* 安装：Frida的Node.js的bindings
  * 说明：
    * 默认不用安装
    * 需要用到时，再去安装即可
  * 安装命令
    ```bash
    npm install frida
    ```

## 安装后

安装frida后，可以查看和确认位置和版本：

举例：

```bash
➜  ~ which frida
/opt/homebrew/bin/frida
➜  ~ frida --version
16.4.5
```
