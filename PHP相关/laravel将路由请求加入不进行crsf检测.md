### laravel将路由请求加入不进行crsf检测

* laravel的路由crsf跨站检测是通过中间件VerityCrsfToken实现的，要更改只需要在$except中加入对应的路由即可。