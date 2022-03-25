# 1.启用buildx


## 环境暴露

```shell
export DOCKER_CLI_EXPERIMENTAL=enabled
```


验证是否开启：
```shell
docker buildx version
```


## 启用 binfmt_misc

> 如果你使用的是 Docker 桌面版（MacOS 和 Windows），默认已经启用了 binfmt_misc，可以跳过这一步。


```shell
docker run --rm --privileged docker/binfmt:66f9012c56a8316f9244ffd7622d7c21c1f6f28d
```
