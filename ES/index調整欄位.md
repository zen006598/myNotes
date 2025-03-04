refer: https://www.elastic.co/guide/en/elasticsearch/reference/current/aliases.html

1. 確認需要修改的 mapping
2. create backup index
3. 建立 新 index
4. reindex index

   1. 建立 backup index (空 index)
   2. 背景跑 不要 timeout

   ```
   POST _reindex?wait_for_completion=false
   {
       "source": {
       "index": "price"
       },
       "dest": {
       "index": "price2"
       },
       "script": {
       "source": "ctx._source.sID = (long)ctx._source.sID"
       }
   }
   ```

5. 新增 alias 加入 新表
   ```
   POST /_aliases
   {
       "actions": [
        {
          "remove": {
            "index": "xxxx",
            "alias": "xxxx"
          }
        },
        {
          "add": {
            "index": "xxxx_v*",
            "alias": "xxxx"
          }
        }
       ]
   }
   ```
6. 刪除舊 index
