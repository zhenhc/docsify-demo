- curl命令查询es:

```shell
curl localhost:9200

curl -X POST 'http://10.0.130.119:9200/_sql' -H "Content-Type: application/json" -d 'select * from client_labels'
```

