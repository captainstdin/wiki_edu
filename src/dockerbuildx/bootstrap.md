# 3.bootstrap启动构建器


```shell
docker buildx inspect mybuilder --bootstrap
```


```shell
docker buildx inspect mybuilder --bootstrap
[+] Building 0.7s (1/1) FINISHED                                                
 => [internal] booting buildkit                                            0.7s
 => => starting container buildx_buildkit_mybuilder0                       0.7s
Name:   mybuilder
Driver: docker-container

Nodes:
Name:      mybuilder0
Endpoint:  unix:///var/run/docker.sock
Status:    running
Platforms: linux/arm64, linux/amd64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/mips64le, linux/mips64, linux/arm/v7, linux/arm/v6
```
