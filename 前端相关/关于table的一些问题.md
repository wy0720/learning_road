#关于table的一些问题

1.table的宽、高设置需要配置两个属性

```php+HTML
table{ table-layout: fixed;word-wrap: break-word;}
```

2.table设置td文字超过宽度则...显示

```html
overflow: hidden; （防止文字横向撑开）
white-space:nowrap;
text-overflow: ellipsis;
```

