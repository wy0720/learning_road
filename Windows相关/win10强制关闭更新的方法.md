### win10强制关闭更新的方法

#### 服务设置

* 禁用Windows Update更新服务
  * 快捷键“WIN+R”
  * 输入“services.msc”打开服务
  * 在服务中找到[Windows Update]一项，将其禁用
* 

#### 注册列表设置

* 快捷键“WIN+R”打开
* 输入“regedit”
* 定位到[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\UsoSvc]；
* 右侧找到“Start”键，右键点击修改，把start值改成16进制，将数值改为“4”，并确定保存数据。



以上的这些操作差不多就可以关闭更新了。

参考资料：http://www.lotpc.com/dnzs/8374_2.html



