## Es查询案例

```
http://47.112.147.187:9200/shop_dev_account/_search?size=5&from=0

{
	"query":{
		"bool": {
	      "must": [
	      	
	        {"match":{"content":"价格实惠"}}
	      ]
	    }
	},
	"sort": [
        { "sort_score":{ "order": "desc" }},
        { "img_num": { "order": "desc" }},
        { "like": { "order": "desc" }}
    ]
}
```

