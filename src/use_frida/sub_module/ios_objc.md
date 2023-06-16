# iOS的ObjC

## 查看参数类型

* objc的函数，第0个参数是id，第1个参数是SEL，真正的参数从args[2]开始
  ```bash
  console.log("Type of arg[2] -> " + new ObjC.Object(args[2]).$className)
  ```
