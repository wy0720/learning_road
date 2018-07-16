### git 本地创建项目，第一次提交项目流程

* 以https方式提交创建项目

  ```shell
  1.在github上创建一个空项目，取号名字，将会得到一个https && ssh的地址，先以https来创建
  2.在本地项目目录下：
    git init 
  3.git add .  
  4.git commit -m '第一次提交'
  5.git remote add origin 刚刚复制的https地址
  6.git push -u origin master
  ```

  ​

