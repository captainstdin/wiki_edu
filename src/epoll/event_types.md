# Select,Event,Libevent


### CK10

坊间传闻，在epoll出世前，QQ用户量剧增，
但是select以及select的高配版本poll都无法解决他们的问题，
于是乎QQ当年的服务器就不得不用UDP协议来避规这个问题，
一直到后来有了epoll，QQ开始逐步在PC客户端中的配置项中允许用户选择UDP服务器或TCP服务器。


### select 
尽管select是POSIX标准。即便是select的高配版本poll，也比epoll差太多太多。

每收到X个（ X >= 1 ) 事件触发的时候，需要你自己foreach每个事件，会造成群惊效应。

+ select可管理的fd的数量是`1024`个

### epoll

每收到X个（ X >= 1 ）事件触发，会告诉你具体。会把具体的Fd给你

+ 理论上可以搞定无上限的fd
+ 只挑出可读写（其实严格意义上还有异常）的活跃的fd，其余的fd不理会
+ 使用MMAP加速内核态数据拷贝
### 比喻

每当一个新的socket(fd)请求 来的时候，会携带 一份资源集(fd list)。
select 或者epoll 会告诉你 某个fd已经可读/写




Libevent官网<https://libevent.org/>
