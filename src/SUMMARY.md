# Summary

- [序言](start.md)
  
---

- [理论]( )
  - [什么是进程](./process/3.md)
  - [线程进程区别](./process/1.md)
  - [stream/socket](./process/2.md)

- [pcntl_*]()
  - [pcntl_exec()](./pcntl_exec/1.md)
  - [pcntl_wait()||monitor](./pcntl_wait/pcntl_wait.md)
  - [pcntl_waitpid()](./pcntl_wait/pcntl_waitpid.md)
  - [pcntl_wtermsig()](./pcntl_wait/pcntl_wtermsig.md)
- [posix_*]()
  - [posix_getpid()](./posix/posix_getpid.md)
  - [posix_getppid()](./posix/posix_getppid.md)
- [进程守护deamon]()
  - [1.创建子进程，终止父进程](./daemon/1.md)
  - [2.子进程中创建新会话](./daemon/2.md)
  - [3.改变工作目录](./daemon/3.md)
  - [4.重设文件创建掩码](./daemon/4.md)
  - [5.关闭文件描述符](./daemon/5.md)
  - [代码案例](./daemon/code.md)
- [数据流stream_socket_*]()
  - [1.案例http/tcp](./stream_socket/http.md)
  
- [IO多路复用]()
  - [1.什么是Epoll](epoll/epoll.md)
  - [Select,Event,Libevent](./epoll/event_types.md)
  - [2.yield与协程](./epoll/yield.md)
  - [socket与event复用](./epoll/socket_event.md)

  
- [docker多端编译]()
  - [1.启用buildx](./dockerbuildx/enalble.md)
  - [2.create构建器](./dockerbuildx/create.md)
  - [3.bootstrap启动构建器](./dockerbuildx/bootstrap.md)
  - [4.构建镜像buildx](./dockerbuildx/buildx.md)
  
- [docker-composer]()
  - [1.参数参考](./docker-composer/argv.md)
  - [2.易支付案例](./docker-composer/epay.md)
