# 2.create

Docker 默认会使用不支持多 CPU 架构的构建器，我们需要手动切换。

```shell
docker buildx create --use --name mybuilder
```
