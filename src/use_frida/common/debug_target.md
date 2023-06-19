# Frida调试目标的方式

## 背景知识

### iOS的进程的类型

iOS的进程有2类：

* `app`=`Bundle`=`软件包`
  * 举例
    * （iOS系统自带的）设置app=包名是：`com.apple.Preferences`
* `Executable`=`可执行文件`=`二进制文件`
  * 举例
    * （AppleMediaServices.framework 的 ）`amsaccountsd`

## frida和frida-trace是调试目标的方式的逻辑是一样的

```bash
frida --help
```

和：

```bash
frida-trace --help
```

都可以看到对应help语法：

```bash
  -f TARGET, --file TARGET
                        spawn FILE
  -F, --attach-frontmost
                        attach to frontmost application
  -n NAME, --attach-name NAME
                        attach to NAME
  -N IDENTIFIER, --attach-identifier IDENTIFIER
                        attach to IDENTIFIER
  -p PID, --attach-pid PID
                        attach to PID
  -W PATTERN, --await PATTERN
                        await spawn matching PATTERN
```

## `frida`/`frida-trace`调试目标的方式

* `frida`/`frida-trace`的调试目标方式 概述
  * 支持2种模式：`Spawn`和`Attach`
    * **Spawn模式**：只有一种写法
      * `-f TARGET`
        * TARGET是 **app包名** 或 **Executable二进制文件名**
    * **Attach模式**：有多种写法=针对app或Executable有不同写法
      * 同时支持**app或Executable**的：`-p PID`
        * PID是app或Executable的进程ID
      * 针对**Executable**的： `-n NAME`
        * NAME是Executable的二进制文件名，比如`amsaccountsd`
      * 针对**app**的：`-N IDENTIFIER`
        * IDENTIFIER是app的包名，比如`com.apple.Prefrences`
      * 特殊的：针对当前手机中正在运行的`frontmost`最前台的：`-F`
        * 由于是，当前最前台的正在运行的= 那只能是带页面显示的app，且也无需再加额外参数指定app

### 详解

`frida`/`frida-trace`的调试目标方式的详细解释：

* Spawn模式：孵化出新进程
  * 典型使用场景：app/Executable**还没运行**，想要从app/Executable刚开始启动就去hook调试
  * 语法
    * `-f TARGET`, `--file TARGET`
      * spawn FILE
  * 举例
    ```bash
    frida -U -f com.apple.Preferences
    ```
  * 说明
    * `TARGET`：是 APP包名 或 Executable二进制文件名？
    * spawn方式启动后，会立刻暂停运行
      * 需要继续运行，需要手动输入：`%resume`
      * 如果不想要每次spawn启动后暂停，则可以加上参数：--no-pause
        * 注：旧版本才有--no-pause，新版本已不支持--no-pause
* Attach模式：挂载到一个已经正在运行的进程
  * 典型使用场景：app/Executable**已经在运行**了，想要hook调试相关逻辑
  * 常见写法
    * 针对于app的
      * `frida -N com.app.identifier`
        * 语法
          * `-N IDENTIFIER, --attach-identifier IDENTIFIER`
            * attach to IDENTIFIER
        * 举例
          ```bash
          frida -U -N com.apple.Preferences
          ```
        * 说明
          * 如何查看到app的包名
            * Mac中用`frida-ps`
              ```bash
              frida-ps -Ua
              ```
              ```bash
              frida-ps -Uai
              ```
      * `frida -F`
        * 语法
          * `-F, --attach-frontmost`
            * attach to frontmost application
        * 举例
          * 当前iPhone中正在显示设置app的界面
            ```bash
            frida -U -F
            ```
        * 说明
          * F=Frontmost=Focused
          * 在操作之前，确保要调试的app，处在前台=正在运行=当前界面
    * 针对于Executable的
      * `frida -n executableFilename`
        * 语法
          * `-n NAME, --attach-name NAME`
            * attach to NAME
        * 举例
          ```bash
          frida -U -n amsaccountsd
          ```
    * 针对于 app 或 Executable 的
      * `frida -p PID`
        * 语法
          * `-p PID, --attach-pid PID`
            * attach to PID
        * 举例
          * 12886是msaccountsd的PID
            ```bash
            frida -U -p 12886
            ```
          * 19641是com.apple.Preferences的进程ID
            ```bash
            frida -U -p 19641
            ```
        * 说明
          * 如何获取PID
            * iPhone中通过ssh运行ps
              ```bash
              ps -A | grep yourAppOrExecutableName
              ```
            * Mac中用frida-ps
              ```bash
              frida-ps -U
              ```
              ```bash
              frida-ps -Ua
              ```

## 注意事项

* 如果调试方式对应参数使用有误，是无法正常调试的
  * 举例
    * 对于app，用`-n`（而不是`-N`），会报错
      * 对于app：系统内置的应用：`Preferences`（包名`com.apple.Preferences`）
        * 虽然也有对应的二进制文件：`/Applications/Preferences.app/Preferences`
      * 但是是无法通过Executable的Attach参数去启动的
      * 即，下面写法是无效的，会报错的：
        ```bash
        ➜  ~ frida-trace -U -n Preferences
        Failed to spawn: unable to find process with name 'Preferences'
        ```
* 如果用`-N`出现异常情况，则可考虑换用`-F`
  * 举例
    * Frida调试设置app
      * 用`-N 包名` 或 `-F`不加包名
        ```bash
        frida-trace -U -N com.apple.Preferences -O Preferences_accoutLogin_hook.txt
        ```
        ```bash
        frida-trace -U -F -O Preferences_accoutLogin_hook.txt
        ```
      * -> 就会导致：设置app异常 -》点击登录iPhone，会出现弹框：接入互联网以登录iPhone，好像是：此时设置app无法联网了
      * 而换用`-F 包名`去调试
        ```bash
        frida-trace -U -F com.apple.Preferences -O Preferences_accoutLogin_hook.txt
        ```
      * -> 就一切正常，设置app工作正常，点击登录iPhone，可以出现Apple ID登录页
