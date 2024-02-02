## Runner节点使用

### 开启gokins插件服务
```bash
# 修改gokins配置文件 ~/.gokins/app.yml
# 提示: 如果安装的时候已经开启了,就不必修改此配置了
server:
    host: http://gokins.cn
    loginKey: tJOm8dk0Qg5W2X2WSdNxr7pwMmecDPiY
    runLimit: 0
    hbtpHost: 0.0.0.0:8031 # 开启插件服务(端口:8031)
    secret: abcd12345678   # 设置插件服务安全密钥(外网最好都设置)
```

### 使用Runner下载gokinsr
命令运行 ./gokinsr -h 192.168.1.2:8031 -s abcd12345678 -p build@golang -p build@node
+ 192.168.1.2 为上面设置的gokins内网地址,可根据需要修改
+ -p 参数用做设置此runner可以运行 [build@golang, build@node] 这两个类型的step
> 官方编译版本下载 http://down.gokins.cn/static/golang/gokins/gokinsr

### 使用镜像+Runner下载gokinsr-alpine
docker使用，环境变量配置
> go环境
```bash
docker run -d --name go-env \
-e GOKINS_SERVHOST=172.17.0.1:8031 \
-e GOKINS_SERVSECRET=maoguorui666 \
-e GOKINS_PLUGIN=build@golang \
-e GOPROXY="https://goproxy.cn,direct" \
-v /usr/local/bin/gokinsr-alpine:/usr/bin/gokinsr \
--entrypoint /usr/bin/gokinsr \
golang:1.16.15-alpine3.15
```
>nodejs环境
```bash
docker run -d --name gokinsr-node \
-e GOKINS_SERVHOST=172.17.0.1:8031 \
-e GOKINS_SERVSECRET=maoguorui666 \
-e GOKINS_PLUGIN=build@node \
-v /usr/local/bin/gokinsr-alpine:/usr/bin/gokinsr \
--entrypoint /usr/bin/gokinsr \
node:14-alpine3.14
```

> 官方编译版本下载 http://down.gokins.cn/static/golang/gokins/gokinsr-alpine
