# 1.参数参考

+ version: "3.7" #docker引擎版本



## build 
```yaml
version: "3.7"
services:
  webapp:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
      labels:
        - "com.example.description=Accounting webapp"
        - "com.example.department=Finance"
        - "com.example.label-with-empty-value"
      target: prod
```

+ context：上下文路径。
+ dockerfile：指定构建镜像的 Dockerfile 文件名。
+ args：添加构建参数，这是只能在构建过程中访问的环境变量。
+ labels：设置构建镜像的标签。
+ target：多层构建，可以指定构建哪一层。



## command
```yaml
command: ["bundle", "exec", "thin", "-p", "3000"]
```

## depends_on
> 以依赖性顺序启动服务。在以下示例中，先启动 db 和 redis ，才会启动 web。

```yaml
version: "3.7"
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
```

## DNS

```yaml
dns:
  - 8.8.8.8
  - 9.9.9.9
```

## ENV

```yaml
env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env
```


## environment

```yaml
environment:
  RACK_ENV: development
  SHOW: 'true'
  "PASSWORD=123456"
```

## network_mode
```yaml
network_mode: "bridge"
#network_mode: "host"
#network_mode: "none"

services:
  some-service:
    networks:
      some-network:
        aliases:
          - alias1
```


## restart
```yaml
restart: "no"
#restart: always
#restart: on-failure #在容器非正常退出时（退出状态非0），才会重启容器。
#restart: unless-stopped #在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器
``` 

##  volumes:
```yml
   volumes:
     - ./mysql:/var/lib/mysql #宿主机：容器
```

{{#include ./epay.md}}
