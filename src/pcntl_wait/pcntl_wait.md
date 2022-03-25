# pcntl_wait || pcntl_waitpid

## pcntl_wait() == 任意子进程

## 定义
```php
pcntl_wait(int &$status, int $options = 0, array &$rusage = ?): int
```
如果一个子进程在调用此函数时已经退出（俗称僵尸进程），此函数立刻返回。  
子进程使用的所有系统资源将被释放  
## 等值演算 
~~~
pcntl_wait == pcntl_waitpid(-1,&$status,0); 
~~~


## return  ()

pcntl_wait() 返回退出的子进程进程号，发生错误时返回 -1,如果提供了 WNOHANG 作为 option（wait3可用的系统）并且没有可用子进程时返回 0。


## 监控子进程&&重启子进程

```php

<?php
$childs = [];
function fork()
{
    $pid = pcntl_fork();

    if ($pid < 0) {
        exit('错误无法创建');
    }

    //子进程执行
    if ($pid == 0) {

        //子进程永远阻塞这里
        while (true) {
            sleep(10);
        }
    }

    //父进程执行
    if ($pid > 0) {
        global $childs;
        $childs[$pid] = $pid;
    }
}

//创建子进程多个
for ($i = 0; $i <= 5; $i++) {
    fork();
}

echo '父进程(' . posix_getpid() . ')检测子进程状态并且回收' . PHP_EOL;
while (count($childs)) {
    $exit_id = pcntl_wait($status);
    if ($exit_id >= 0) {
        $msg = pcntl_wtermsig($status);
        echo sprintf('子进程： (%s) 退出，原因：%s', $exit_id, $msg) . PHP_EOL;
        //删除数组资源句柄
        unset($childs[$exit_id]);
    }
    //monitor 监控子进程
    //子进程数量不足2个，就fork()
    if(count($childs)<2){
        fork();
    }
}

echo "Done\n";
```

{{#include ./zombie_process.md}}


