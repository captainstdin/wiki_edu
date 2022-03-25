# code

```php
<?php
$childs = [];


function deamon()
{
    $pid = pcntl_fork();
    if ($pid < 0) {
        exit('错误无法创建');
    }
    //子进程执行
    if ($pid == 0) {
        echo sprintf('子进程(%s)被创建', $pid) . PHP_EOL;
        //2.让子进程会话变成主会话
        $sid=posix_setsid();
        if($sid<=0){
            die('set fail');
        }

        //3.
        if(chdir('/')===false){
            die('fail to change dir ');
        }

        //4.与chmod相反 默认继承了父进程的文件创建掩码
        umask(0);

        //5.默认继承了父进程打开的文件
        fclose(STDIN);
        fclose(STDOUT);
        fclose(STDERR);
        while (true) {
            sleep(10);
        }
    }

    //父进程执行
    if ($pid > 0) {
       //1.2 父进程退出，
        //2让子进程会话变成主会话
        exit();
    }
}
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

//初始化创建5个子进程
for ($i = 0; $i <= 5; $i++) {
    fork();
}

echo '父进程(' . posix_getpid() . ')检测子进程状态并且回收' . PHP_EOL;
while (count($childs)) {
    $exit_id = pcntl_wait($status);
    if ($exit_id >= 0) {
        $msg = pcntl_wtermsig($status);
        //删除数组资源句柄
        echo sprintf('子进程： (%s) 退出，原因：%s', $exit_id, $msg) . PHP_EOL;
        unset($childs[$exit_id]);
    }
    //monitor 监控子进程
    //子进程数量不足2个，就fork()
    if (count($childs) < 2) {
        fork();
    }
}

```

