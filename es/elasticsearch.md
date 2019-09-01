## elasticsearch

* 删除索引下的文件

  ```json
  post http://47.112.147.187:9200/shop_dev_shop_vip_search/_delete_by_query
  {
  "query":{"match_all":{}}
  }
  ```

  