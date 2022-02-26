1. bootstrap.yml

```yaml
spring:
  application:
    name: scrm-admin-app
  cloud:
    nacos:
      discovery:
        server-addr: 10.3.24.133:8848
        namespace: 1c2b9cf3-718c-4f86-8b43-53e800c90c29
      config:
        #server-addr: 10.3.24.133:8848
        #file-extension: yaml
        prefix: scrm-main-app
        bootstrap:
          # 开启预加载配置
          enable: true
        # 服务地址
        server-addr: 10.3.24.133:8848
        # 服务账号
        username: nacos
        # 服务密码
        password: nacos
        # data-id
        data-id: scrm-main-app
        # group
        group: scrm-main-app
        # 命名空间
        # namespace: '刚刚自己新建的命名空间ID，没有新建不需要配置namaspace'
        namespace: 1c2b9cf3-718c-4f86-8b43-53e800c90c29
        # 配置文件类型
        type: yaml
        file-extension: yaml
        # 最大重试次数
        max-retry: 10
        # 自动刷新
        auto-refresh: true
        # 重试时间
        config-retry-time: 2000
        # 监听长轮询超时时间
        config-long-poll-timeout: 46000
```
