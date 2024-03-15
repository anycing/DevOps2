# 网站存活监控

### web\_monitor.sh

```bash
#!/bin/bash

source /etc/init.d/functions

usage() {
    echo "Usage: $0 URL"
}

checkWeb() {
    wget --spider -q -T 5 --tries=1 $1

    if [ "$?" -eq 0 ]; then
        action "$1 is running." /bin/true
    else
        action "$1 is down." /bin/false
    fi
}

main() {
    if [ "$#" -ne 1 ]; then
        usage
        exit 2
    fi
    checkWeb $1
}

main $*
```

<figure><img src="../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>
