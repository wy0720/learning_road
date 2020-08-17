# this在这类function中的指向

[https://segmentfault.com/a/1190000004639145]()

this在javascript中真的是无法让我们捉摸透彻。但是有一个小窍门，就是：<font color=red>一般情况下，this指向的是当前实例化对象，如果没有找到该对象，则是指向window。</font>从使用上来讲，我们应该排除new Function的讨论，因为它和我们常用的函数声明是一致的。

**普通的函数中this的指向**

函数声明的时候，如果使用了this，那么就要看是把该函数当做一个对象加以返回，还是以仅执行函数体。普通函数执行时，我们完全没有引入对象、类这些概念，因此，this指向window。通过代码来看下：

```
var msg;
function message(msg) {
    this.msg = msg;
}
message('ok');
alert(msg);
```

首先是声明一个函数message，在函数中this.msg实际上就是window.msg，也实际上就是代码开头的msg。因此，当执行完message('ok')的时候，开头的全局变量msg也被赋值为ok。

**通过function构造类时this的指向**

如果function被构造为一个类，那么必然存在该类被实例化的一个过程，如果没有实例化，那么该类实际上并没有在程序中被使用。而一旦实例化，那么this将指向实例化的对象。

```
var age = 3;
var cat1 = new function() {
    this.name = 'Tom';
    this.age = 2;
    this.weight = function(age) {
        var age = age * 2;
        var _age = this.age * 2;
        return 'weight by age:' + age + '; weight by this.age:' + _age;
    }(this.age);
    this.eye = new function() {
        this.size = '1.5cm';
        this.color = 'red';
    };
    this.catching = function(mouse) {
        return this.name + ' is catching ' + mouse;
    };
};
alert(cat1.weight);
alert(cat1.eye.color);
alert(cat1.catching('Jerry'));
```

上面代码中标记了4处红色的this的使用。根据我们的原则，this指向实例化对象，我们来对每一个this进行分解。

首先是cat1.weight，我使用了function(){}()，直接利用猫咪的年龄进行计算得出体重返回给weight属性。

第一个this.age出现在function(){}(this.age)，这个this.age实际上是一个传值过程，如果你对我之前分析function(){}()比较了解的话，应该知道，this.age实际上是和前面this.age = 2指同一个，这里的this.age的this，首先要去找它所在的function，然后看这个function是否被实例化，最后确认，确实被实例化为cat1，因此this=cat1。

第二个this.age出现在function(){this.age}()。同样，你先需要对function(){}()再次深入了解，实际上，function(){}()就是执行一个函数而已，我们前面提到了，普通函数执行中this=window，所以，这里的this.age实际上是var age = 3。

第三个this.color出现在new function(){this.color}，这里就比较好玩，由于有一个new，实际上也被实例化了，只不过是对匿名类的实例化，没有类名，而且实例化仅可能出现这一次。因此，this.color的this要去找new function的主人，也就是this.eye，而this.eye的this=cat1，所以cat1.eye.color='red'。

第四个this.name出现在function(){this.name}，它出现在cacthing方法中，它既不是普通的函数执行，也不是实例化为对象，而是正常的类中的方法的声明，因此this指向要去找它所在的function被实例化的对象，也就是cat1。





###  文章二

[https://www.cnblogs.com/pssp/p/5216085.html]()

首先必须要说的是，**this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁**，**实际上this的最终指向的是那个调用它的对象（**这句话有些问题，后面会解释为什么会有问题，虽然网上大部分的文章都是这样说的，虽然在很多情况下那样去理解不会出什么问题，但是实际上那样理解是不准确的，所以在你理解this的时候会有种琢磨不透的感觉**）**，那么接下来我会深入的探讨这个问题。

　　为什么要学习this？如果你学过面向对象编程，那你肯定知道干什么用的，如果你没有学过，那么暂时可以不用看这篇文章，当然如果你有兴趣也可以看看，毕竟这是js中必须要掌握的东西。

 

**例子1：**

```
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();
```

按照我们上面说的this最终指向的是调用它的对象，这里的函数a实际是被Window对象所点出来的，下面的代码就可以证明。

```
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this);　　//Window
}
window.a();
```

和上面代码一样吧，其实alert也是window的一个属性，也是window点出来的。

**例子2：**

```
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);  //追梦子
    }
}
o.fn();
```

　　这里的this指向的是对象o，因为你调用这个fn是通过o.fn()执行的，那自然指向就是对象o，这里再次强调一点，this的指向在函数创建的时候是决定不了的，在调用的时候才能决定，谁调用的就指向谁，一定要搞清楚这个。

 

其实例子1和例子2说的并不够准确，下面这个例子就可以推翻上面的理论。

如果要彻底的搞懂this必须看接下来的几个例子

本文出处：[追梦子博客](http://www.cnblogs.com/pssp/)

**例子3：**

```
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子
    }
}
window.o.fn();
```

　　这段代码和上面的那段代码几乎是一样的，但是这里的this为什么不是指向window，如果按照上面的理论，最终this指向的是调用它的对象，这里先说个而外话，window是js中的全局对象，我们创建的变量实际上是给window添加属性，所以这里可以用window点o对象。

　　这里先不解释为什么上面的那段代码this为什么没有指向window，我们再来看一段代码。

```
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12
        }
    }
}
o.b.fn();
```

　　这里同样也是对象o点出来的，但是同样this并没有执行它，那你肯定会说我一开始说的那些不就都是错误的吗？其实也不是，只是一开始说的不准确，接下来我将补充一句话，我相信你就可以彻底的理解this的指向的问题。

　　情况1：如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window，这里需要说明的是在js的严格版中this指向的不是window，但是我们这里不探讨严格版的问题，你想了解可以自行上网查找。

　　情况2：如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象。

　　情况3：如果一个函数中有this，**这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象，**例子3可以证明，如果不相信，那么接下来我们继续看几个例子。

```
var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();
```

尽管对象b中没有属性a，这个this指向的也是对象b，因为this只会指向它的上一级对象，不管这个对象中有没有this要的东西。

**还有一种比较特殊的情况，例子4：**

```
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```

这里this指向的是window，是不是有些蒙了？其实是因为你没有理解一句话，这句话同样至关重要。

　　this永远指向的是最后调用它的对象，也就是看它执行的时候是谁调用的，例子4中虽然函数fn是被对象b所引用，但是在将fn赋值给变量j的时候并没有执行所以最终指向的是window，这和例子3是不一样的，例子3是直接执行了fn。

　　this讲来讲去其实就是那么一回事，只不过在不同的情况下指向的会有些不同，上面的总结每个地方都有些小错误，也不能说是错误，而是在不同环境下情况就会有不同，所以我也没有办法一次解释清楚，只能你慢慢地的去体会。

 

**构造函数版this：**

```
function Fn(){
    this.user = "追梦子";
}
var a = new Fn();
console.log(a.user); //追梦子
```

　　这里之所以对象a可以点出函数Fn里面的user是因为new关键字可以改变this的指向，将这个this指向对象a，为什么我说a是对象，因为用了new关键字就是创建一个对象实例，理解这句话可以想想我们的例子3，我们这里用变量a创建了一个Fn的实例（相当于复制了一份Fn到对象a里面），此时仅仅只是创建，并没有执行，而调用这个函数Fn的是对象a，那么this指向的自然是对象a，那么为什么对象a中会有user，因为你已经复制了一份Fn函数到对象a中，用了new关键字就等同于复制了一份。

　　除了上面的这些以外，我们还可以自行改变this的指向，关于自行改变this的指向请看[JavaScript中call,apply,bind方法的总结](http://www.cnblogs.com/pssp/p/5215621.html)这篇文章，详细的说明了我们如何手动更改this的指向。

 

**更新一个小问题当this碰到return时**

```
function fn()  
{  
    this.user = '追梦子';  
    return {};  
}
var a = new fn;  
console.log(a.user); //undefined
```

再看一个

```
function fn()  
{  
    this.user = '追梦子';  
    return function(){};
}
var a = new fn;  
console.log(a.user); //undefined
```

再来

```
function fn()  
{  
    this.user = '追梦子';  
    return 1;
}
var a = new fn;  
console.log(a.user); //追梦子
function fn()  
{  
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a.user); //追梦子
```

什么意思呢？

　　**如果返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数的实例。
**

```
function fn()  
{  
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a); //fn {user: "追梦子"}
```

　　还有一点就是虽然null也是对象，但是在这里this还是指向那个函数的实例，因为null比较特殊。

```
function fn()  
{  
    this.user = '追梦子';  
    return null;
}
var a = new fn;  
console.log(a.user); //追梦子
```

**知识点补充：**

　　1.在严格版中的默认的this不再是window，而是undefined。

　　2.new操作符会改变函数this的指向问题，虽然我们上面讲解过了，但是并没有深入的讨论这个问题，网上也很少说，所以在这里有必要说一下。**
**

```
function fn(){
    this.num = 1;
}
var a = new fn();
console.log(a.num); //1
```

　　为什么this会指向a？首先new关键字会创建一个空的对象，然后会自动调用一个函数apply方法，将this指向这个空对象，这样的话函数内部的this就会被这个空的对象替代。

**2017-09-15 11:49:14**

　　注意: 当你new一个空对象的时候,js内部的实现并不一定是用的apply方法来改变this指向的,这里我只是打个比方而已.

　　if (this === 动态的\可改变的) return true;



### 总结

总结（5点）
 1.fn() 里面的 this 就是 window（非严格模式下）
 2.fn() 是 strict mode，this 就是 undefined
 3.a.b.c.fn() 里面的 this 就是 a.b.c
 4.new F() 里面的 this 就是新生成的实例
 5.() => console.log(this) 里面 this 跟外面的 this 的值一模一样
 当然还有事件监听的时候，this是监听元素，setTimeout的函数内this是window（非严格模式下）。
 其实**万变不离其宗**。一句话：
 **this是一个动态的概念，当它所在的函数被调用的时候才能确定，就是call或者aply的第一个参数，是它被调用时的对象环境**

