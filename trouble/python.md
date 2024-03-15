# python

### 使用 Python 创建僵尸进程

```python
#!/usr/bin/env python
#-*-encoding:UTF-8-*-
import os
import time
import subprocess
if __name__ == '__main__':
    p = subprocess.Popen('ls',shell=True,close_fds=True,bufsize=-1,stdout=subprocess.PIPE,stderr=subprocess.STDOUT)
    file =  p.stdout.readlines()
    for i in range(0, len(file)):
        print file[i]
    #end for
    while True:
        time.sleep(1)
    #end while
#end if
```
