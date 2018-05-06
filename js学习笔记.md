# js学习笔记

## 1.实现导航栏状态条的jQuery代码

```html
<div class="modal fade navbar-small" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
   <ul class="nav navbar-nav nav-font">
        <li class="first active"><a href="index.php"><span>主</span>页</a><img src="images/header/nav-line.png" ></li>
        <li class=""><a href="story.php">小勺故事</a><p></p></li>
        <li class=""><a href="product.php">产品购买</a><p></p></li>
        <li class=""><a href="feedback.php">客户口碑</a><p></p></li>
        <li class=""><a href="share.php">新鲜分享</a><p></p></li>
   </ul>
</div>
```

```javascript
        $(document).ready(function(){
            $(".nav li a").each(function(){
                if ($($(this))[0].href == String(window.location)) {
                    $(this).parent().addClass("active");
                    $(this).parent().siblings().removeClass("active");
                }
            });
        });
```
## php跳出ifream

```php
echo("<SCRIPT LANGUAGE=\"JavaScript\">
					        alert(\"操作成功,请重新登录\");
					        top.location.href=\"?m=admin&c=Index&a=login\";
					        </SCRIPT>");
```

## 图片未加载成功时加载默认图片

```javascript
  function nofind(){
    var img=event.srcElement;
    img.src="__WEB_SITE__public/img/news/default.jpg";
    img.onerror=null; //控制不要一直跳动
  }
```

 