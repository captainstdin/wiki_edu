# 使用Event边缘触发执行Socket处理



```php
<?php
define( "DS", DIRECTORY_SEPARATOR );
define( "ROOT", __DIR__ );
spl_autoload_register( function( $s_class_name ) {
    $s_path = str_replace( "\\", "/", $s_class_name );
    $s_file = ROOT.DS.$s_path.'.php';
    require_once $s_file;
} );


// 创建一个非阻塞的listen-socket资源
$r_listen_socket = socket_create( AF_INET, SOCK_STREAM, SOL_TCP );
socket_set_option( $r_listen_socket, SOL_SOCKET, SO_REUSEADDR, 1 );
socket_set_option( $r_listen_socket, SOL_SOCKET, SO_REUSEPORT, 1 );
socket_bind( $r_listen_socket, '0.0.0.0', 6666 );
socket_listen( $r_listen_socket );
socket_set_nonblock( $r_listen_socket );


// 创建一个Event-Loop
// 然后将上面的listen-socket加入到Event-Loop中
// 这里就已经比较直白了，只要你要创建一个资源
// 这个资源可以是socket、可以是stream等等
// 因为Event-Loop已经被我们抽象称了模块
// 所以你创建的资源只要支持Event，那么都很方便地通过Event-Loop模块写出来
$o_event_loop = new Event\Libevent();
$o_event_loop->add( $r_listen_socket, Event\Libevent::EV_READ, function() use( $r_listen_socket, $o_event_loop ) {
    $r_connection_socket = socket_accept( $r_listen_socket );
    $o_event_loop->add( $r_connection_socket, Event\Libevent::EV_WRITE, function() use ( $r_connection_socket, $o_event_loop ) {
        $s_data = "Hello HTTP World!";
        $s_html = "HTTP/1.1 200 OK\r\nContent-Length: ".strlen( $s_data )."\r\n\r\n{$s_data}\n";
        $i_written = socket_write( $r_connection_socket, $s_html, strlen( $s_html ) );
        $o_event_loop->del( $r_connection_socket, Event\Libevent::EV_ALL );
    });
} );
$o_event_loop->loop();
```

1. 第一步：在PHP里各种能支持Event-Loop的资源，比如socket、比如stream
2. 第二步：初始化Event-Loop，然后将第一步里创建好的资源扔到Event-Loop里
3. 第三步：完成

参考
<https://cloud.tencent.com/developer/article/1596705>



```php
<?php
namespace Event;
class Libevent {
    const  EV_ALL   = 1;
    const  EV_READ  = 2;
    const  EV_WRITE = 4;
    const  EV_EXCEPTION    = 8;
    public $o_event_config = null;
    public $o_event_base   = null;
    /*
     * @desc : 保存事件event
     * */
    public $a_event  = array();
    public $a_client = array();
    public function __construct() {
        $o_event_config = new \EventConfig();
        $o_event_base   = new \EventBase( $o_event_config );
        $this->o_event_config = $o_event_config;
        $this->o_event_base   = $o_event_base;
    }
    /*
     * @desc  : 添加一个事件
     * @param : socket fd
     * @param : event type, EV_READ EV_WRITE
     * @param : callback
     * */
    public function add( $r_fd, $i_event_type, $f_callback ) {
        $i_event_flag = $i_event_type == self::EV_READ ? \Event::READ | \Event::PERSIST : \Event::WRITE | \Event::PERSIST ;
        $o_event = new \Event( $this->o_event_base, $r_fd, $i_event_flag, $f_callback );
        $o_event->add();
        $i_fd = intval( $r_fd );
        $this->a_event[ $i_fd ][ $i_event_type ] = $o_event;
        $this->a_client[ $i_fd ] = $r_fd;
        //echo json_encode( $this->a_client )." | ".json_encode( $this->a_event ).PHP_EOL;
    }
    /*
     * @desc : 删除一个事件
     * */
    public function del( $r_fd, $i_event_type ) {
        $i_fd = intval( $r_fd );
        // 删除所有事件类型
        if ( self::EV_ALL === $i_event_type ) {
            if ( isset( $this->a_event[ $i_fd ] ) ) {
                if ( isset( $this->a_event[ $i_fd ][ self::EV_WRITE ] ) ) {
                    echo "del write".PHP_EOL;
                    $o_event = $this->a_event[ $i_fd ][ self::EV_WRITE ];
                    $o_event->free();
                    unset( $this->a_event[ $i_fd ][ self::EV_WRITE ] );
                }
                if ( isset( $this->a_event[ $i_fd ][ self::EV_READ ] ) ) {
                    echo "del read".PHP_EOL;
                    $o_event = $this->a_event[ $i_fd ][ self::EV_READ ];
                    $o_event->free();
                    unset( $this->a_event[ $i_fd ][ self::EV_READ ] );
                }
                if ( isset( $this->a_event[ $i_fd ][ self::EV_EXCEPTION ] ) ) {
                    echo "del exception".PHP_EOL;
                    $o_event = $this->a_event[ $i_fd ][ self::EV_EXCEPTION ];
                    $o_event->free();
                    unset( $this->a_event[ $i_fd ][ self::EV_EXCEPTION ] );
                }
            }
        }
        // 删除指定事件类型
        else {
            if ( isset( $this->a_event[ $i_fd ] ) ) {
                if ( isset( $this->a_event[ $i_fd ][ $i_event_type ] ) ) {
                    $o_event = $this->a_event[ $i_fd ][ $i_event_type ];
                    $o_event->free();
                    unset( $this->a_event[ $i_fd ][ $i_event_type ] );
                }
            }
        }
        unset( $this->a_client[ $i_fd ] );
    }
    /*
     * @desc : 陷入事件循环
     * */
    public function loop() {
        $this->o_event_base->loop();
    }
}
```
