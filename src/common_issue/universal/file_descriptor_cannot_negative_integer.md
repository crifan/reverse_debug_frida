# ValueError: file descriptor cannot be a negative integer

* 问题：`frida-ps`、`frida-ls-devices`等frida-tools工具运行时报错：`ValueError: file descriptor cannot be a negative integer (-42)`
* 原因：当前`12.0.3`的`frida-tools`有bug
* 解决办法：升级到最新版`frida-tools`
* 具体步骤：
  ```bash
  pip install --upgrade frida_tools
  ```
  * 注：查看当前frida-tools的版本：
    ```bash
    pip show frida_tools
    ```
