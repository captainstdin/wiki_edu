# pcntl_waitpid

```php
pcntl_waitpid(int $pid, int &$status, int $options = 0): int
```



## options

~~~
define('WNOHANG', 1);如果没有子进程退出立刻返回
define('WUNTRACED', 2);子进程已经退出并且其状态未报告时返回
~~~


```php

<?php
for ((int)$i = 0; $i < 4; $i++) {
    $_process_id = \pcntl_fork();

    if ($_process_id == -1) {
        $this->print_r('创建子进程失败 for_id:' . $i);
        continue;
    }

    if($_process_id >=1){
        /**
         * 父进程会得到子进程号，所以这里是父进程执行的逻辑
         * WNOHANG 子进程没退出，不阻塞
         * WUNTRACED 子进程没退出，就阻塞,如果子进程退出，就返回子进程ID
         * 等待子进程中断，防止子进程成为僵尸进程。
        */
        $exit_process_id=\pcntl_waitpid($_process_id,$status,WUNTRACED);

        if($exit_process_id>0){

            $this->print_r(sprintf('子进程ID（%s） 状态:%s',$exit_process_id,$status));
        }
        continue;
    }

    sleep(3);
    //子进程 执行的逻辑
    $this->print_r(posix_getppid().'子进程PID'.posix_getpid().' | hello');

    exit(0);
}
```
{{#include ./zombie_process.md}}
