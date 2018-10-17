### ajax data键为变量的方法

```javascript
var key = 'nick';
var value = 123;
var data = {}
data[key] = value

$.ajax({
    ...
    data:data,
    ...
})
```

