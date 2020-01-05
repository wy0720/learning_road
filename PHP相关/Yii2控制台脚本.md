### Yii2控制台脚本

# [yii2 / Console - yii2开发控制台应用时的还我漂漂拳](https://segmentfault.com/a/1190000013364677)



之所以放到此专栏主要是该文章对我们学习yii2-queue有很大帮助。

> 我们经常使用yii2开发命令行应用，也会遇到比如让用户在命令行输入信息、选择等操作，今天为大家介绍的这个yii2的Console类轻松的帮你搞定这件事情，有了它，你的命令行应用将更加高逼格。

为了学习更简单，我们用问答的形式开始。

# Console在哪里？

Console是yii2的一个帮助类，所在文件夹为 **/vendor/yiisoft/yii2/helpers**，里面有 **BaseConsole** 和 **Console** ，Console类继承于BaseConsole，BaseConsole不允许被调用，因此我们都是使用Console类。

在代码里要使用套路

```
use yii\helpers\Console;
...
Console::xxxx()
```

# Hello World

当我们开发yii2的控制台应用的时候，可以echo一个字符串，这样命令行会显示，那么用Console如何实现那？看代码

```
public function actionIndex() {
    Console::output(“Hello World”);
}
```

对，用Console::output,你可能会说，“这还没有echo简单那？”，那么我们接着看。

# 五彩缤纷的Hello World

我现在想让Hello是黄色的，World是绿色的，怎么玩？

```
public function actionIndex() {
  $hello = Console::ansiFormat("Hello",[Console::FG_YELLOW]);
  $world = Console::ansiFormat("World",[Console::FG_GREEN]);
  Console::output("{$hello} {$world}");
}
```

**Console::ansiFormat**可以为字符串增加更多样式。

![图片描述](https://segmentfault.com/img/bV4eVa?w=1154&h=530)

关键在于ansiFormat的第二个参数，这是一个数组，除了能传递FG_开头的前景色，还能传递BG_开头的背景色，也就是说我们不但能改变字体的颜色，还能加背景，比如接下来的这行代码

```
$hello = Console::ansiFormat("Hello",[Console::FG_YELLOW,Console::BG_BLUE]);
```

看图说话

![图片描述](https://segmentfault.com/img/bV4eVd?w=1154&h=530)

那么Console一共支持多少种前景色和背景色那？

- **前景色** FG_BLACK / FG_RED / FG_GREEN / FG_YELLOW / FG_BLUE / FG_PURPLE / FG_CYAN / FG_GREY
- **背景色** BG_BLACK / BG_RED / BG_GREEN / BG_YELLOW / BG_BLUE / BG_PURPLE / BG_CYAN / BG_GREY

# 哥，内容能加粗么？

必须的了，你以为就是颜色么？下面把其他样式再说下，比如加粗、斜体等等，看代码

```
Console::output(Console::ansiFormat("normal",[Console::NORMAL]));
Console::output(Console::ansiFormat("bold",[Console::BOLD]));
Console::output(Console::ansiFormat("italic",[Console::ITALIC]));
Console::output(Console::ansiFormat("underline",[Console::UNDERLINE]));
Console::output(Console::ansiFormat("negative",[Console::NEGATIVE]));
```

如果你的英文不达标可以看效果图

![图片描述](https://segmentfault.com/img/bV4eVi?w=1154&h=530)

# 我想得到用户输入的

没错，这是一个程序最基本的功能，Console当然也可以，就像下面的程序，我们让用户输入名字，然后再输出。

```
$name = Console::input("请输入你的名字：");

$formatName = Console::ansiFormat($name,[Console::FG_YELLOW]);
Console::output("你的名字是：{$formatName}");
```

用 **Console::input**，还能添加提示语，看看结果是不是很有趣。

![图片描述](https://segmentfault.com/img/bV4eVj?w=1154&h=530)

# 用户写太危险，我要他选择。

你说的有道理，你是要用户进行单选题是吧，比如性别等。

```
$sex = Console::select("性别：",[1=>'男',2=>'女']);

$formatSex = Console::ansiFormat($sex,[Console::FG_YELLOW]);
Console::output("你的性别是：{$formatSex}");
```

**Console::select** 施展着小魔法

![图片描述](https://segmentfault.com/img/bV4eVk?w=1154&h=530)

# YES OR NO

让你选择yes还是no，这种情况在我们使用命令行安装东西的时候经常遇到，用yii2的Console如何实现这样的需求那？只需要一个 **Console::confirm** 方法。

```
if (Console::confirm("Are you sure?")) {
    Console::output("user input yes");
}else{
    Console::output("user input no");
}
```

哇哦，逻辑判断又出来了，Console帮你实现着计算机软件的逻辑。

![图片描述](https://segmentfault.com/img/bV4eVm?w=1154&h=530)

聪明的你一定发现了吧，如果你不输入直接回车则在上面的代码中表示你选择了no，具体是yes还是no取决于confirm的第二个参数是true还是false。

# 还有更你x的么？

那就说一个，上面我们说了用户输入是不靠谱的，因此Console也提供了一个验证方法，看代码

```
Console::prompt("你的名字",['required'=>true,'error'=>'必须输入']);
```

prompt很像yii2中ar的rules方法，对每个输入进行验证，通过才能继续下面的代码，比如上面的代码是说**必须填写**，否则抛出错误信息并让你继续输入

![图片描述](https://segmentfault.com/img/bV4eVn?w=1154&h=530)

关于prompt方法主要是第二个参数进行限制，这是一个数组类型的参数，里面的值为

- required 真假，是否必须填写
- default 默认值
- pattern 正则匹配
- validator 自定义验证函数
- error 错误信息

够强大的了吧。

# 最后一个巨牛逼的

为了亮瞎你的眼，我觉得先上效果图

![图片描述](https://segmentfault.com/img/bV4eVo?w=1154&h=530)

帅气吧，你在linux系统上应该不止一次看到它吧，用Console也可以轻松实现

```php
Console::startProgress(0, 1000);
for ($n = 1; $n <= 1000; $n++) {
    usleep(1000);
    Console::updateProgress($n, 1000);

}
Console::endProgress();
```

startProgress、updateProgress、endProgress。

三个你一眼可以看懂的函数就可以搞定，具体什么时候使用，发挥你强大的想象力吧。

# 小结

多么有趣的Console类,当然其实还有很多小方法，比如清空命令行，屏幕尺寸、字符串长度等等，总之你的控制台离不开它。