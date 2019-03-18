### 跑golang项目

* 项目中有依赖，使用gopm来装：https://www.jianshu.com/p/a7c3aeb0948d

* 步骤

  * 安装依赖

  ```shell
  gopm get -g -v -u
  ```

  * 安装项目

  ```shell
  go install
  ```

  * 编译项目

  ```shell
  go build
  ```

  * 运行项目

  ```shell
  go run main.go
  ```

  ​