# case

### 分支

case 语句和 select 语句可以构成分支

```bash
case "$变量" in
    "情况1" )
        命令...
        ;;
    "情况2" )
        命令...
        ;;
    * )
        命令...
        ;;
esac
```

#### Demo

```bash
#!/bin/bash

case "$1" in
    "start" | "START" )
        echo "$0 starting"
    ;;
    "stop" )
        echo "$0 stopping"
    ;;
    "restart" | "reload")
        echo "$0 restarting"
    ;;
    * )
        echo "Usage: $0 {start|stop|restart|reload}"
    ;;
esac
```
