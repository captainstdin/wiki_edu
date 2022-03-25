# 4.构建镜像buildx


> 需要提前通过 docker login 命令登录认证 Docker Hub。 


```shell
docker buildx build -t yangchuansheng/hello-arch --platform=linux/arm,linux/arm64,linux/amd64,linux/386,linux/arm/v7 . --push
```
