### gopm安装使用

* 安装

  ```shell
  go get -v -u github.com/gpmgo/gopm
  ```

* 使用

  ```shell
  # 查看当前工程依赖
  gopm list
  # 显示依赖详细信息
  gopm list -v
  # 列出文件依赖
  gopm list -t [file]
  # 拉取依赖到缓存目录
  gopm get -r xxx
  # 仅下载当前指定的包
  gopm get -d xxx
  # 拉取依赖到$GOPATH
  gopm get -g xxx
  # 检查更新所有包
  gopm get -u xxx
  # 拉取到当前所在目录
  gopm get -l xxx
  # 运行当前目录程序
  gopm run
  # 生成当前工程的 gopmfile 文件用于包管理
  gopm gen -v
  # 根据当前项目 gopmfile 链接依赖并执行 go install
  gopm install -v
  # 更新当前依赖
  gopm update -v
  # 清理临时文件
  gopm clean
  # 编译到当前目录
  gopm bin

  作者：木猫尾巴
  链接：https://www.jianshu.com/p/a7c3aeb0948d
  來源：简书
  简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
  ```

  ​

