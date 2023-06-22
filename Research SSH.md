```bash
# Unbound configuration file
# Cache myapi.com
local-zone: "myapi.com" static
local-data: "myapi.com A 127.0.0.1"
# Forward all other queries to recursive dns server
forward-zone:
    name: "." 
    forward-addr: <recursive dns server-ip>
```

One possible way to forward yourapi.com https requests with unbound and ssh tunnel is to use remote port forwarding. [This is a technique that allows you to create an outbound SSH connection from your local machine to a remote server and then forward traffic from a port on the remote server to a port on your local machine](https://goteleport.com/blog/ssh-tunneling-explained/)[1](https://goteleport.com/blog/ssh-tunneling-explained/).

For example, you could do something like this:

```
ssh -R 443:localhost:80 user@yourapi.com
```

This would create an SSH tunnel from port 443 on yourapi.com to port 80 on your local machine. [Then you could configure unbound to forward queries for yourapi.com to localhost:443 using the `forward-zone` directive](https://www.golinuxcloud.com/setup-ssh-port-forwarding/)[2](https://www.golinuxcloud.com/setup-ssh-port-forwarding/). For example:

```
forward-zone:
    name: "yourapi.com"
    forward-addr: 127.0.0.1@443
```

This way, when you send an https request for yourapi.com, it will be forwarded by unbound to localhost:443, which will be tunneled by SSH to port 80 on your local machine.


> You can use multiple port forwarding in one commmand!
```shell
ssh -p 22 nemo@192.168.1.220 -L 4450:192.168.1.240:445 \ -L 135:192.168.1.240:135 \  
-L 4646:192.168.1.240:49188
```
Learn later :

- Metasploit Framework
- Meterpreter payload and throwing exploits

Fordwarding ports :

```shell
# Box 1
  ~ ❯ ssh jb1 -L 2222:155.138.246.205:22                                                                4m 10s 14:44:35
Last login: Sat Mar 11 22:40:35 2023 from 24.113.238.58
dr1stx@jumpbox1:~$ netstat -nat
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.1:8388          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:5000          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8001          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN
tcp        0      0 144.202.73.118:22       166.176.187.17:47615    ESTABLISHED
tcp        0      0 144.202.73.118:22       31.94.21.57:54872       ESTABLISHED
tcp        0      0 144.202.73.118:39326    155.138.246.205:22      ESTABLISHED
tcp        0      0 144.202.73.118:22       24.113.238.58:65289     ESTABLISHED
tcp        0      0 144.202.73.118:37640    155.138.246.205:22      TIME_WAIT
tcp6       0      0 ::1:8388                :::*                    LISTEN
dr1stx@jumpbox1:~$
```

```shell
# Box 2
  ~ ❯ ssh -p 2222 -i ~/.ssh/dr1stx.key dr1stx@127.0.0.1 -L 2223:144.202.79.65:22                         4m 8s 14:44:48
Last login: Sat Mar 11 22:40:35 2023 from 144.202.73.118
dr1stx@jumpbox2:~$ netstat -nat
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8388          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8001          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN
tcp        0    288 155.138.246.205:22      144.202.73.118:39326    ESTABLISHED
tcp        0      0 155.138.246.205:35576   144.202.79.65:22        TIME_WAIT
tcp        0      0 155.138.246.205:37414   144.202.79.65:22        ESTABLISHED
tcp6       0      0 ::1:8388                :::*                    LISTEN
dr1stx@jumpbox2:~$
```

```shell
# Box 3
  ~/.config/fish ❯ ssh -p 2223 -i ~/.ssh/dr1stx.key dr1stx@127.0.0.1 -L 2224:108.61.222.139:22   ✘ 127  2m 42s 14:44:48
Last login: Sat Mar 11 22:42:50 2023 from 155.138.246.205
dr1stx@jumpbox3:~$ netstat -nat
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.1:8001          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8000          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:8388          0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0   1081 144.202.79.65:22        61.177.173.51:43265     FIN_WAIT1
tcp        0      0 144.202.79.65:22        61.177.173.46:37981     ESTABLISHED
tcp        0    280 144.202.79.65:22        155.138.246.205:37414   ESTABLISHED
tcp6       0      0 ::1:8388                :::*                    LISTEN
dr1stx@jumpbox3:~$

```

Executing -J with single command

```shell
# note the jumps using public IP
# using another user in .1.50 dory
# then forward port locally to another place
ssh -J dr1stx@144.202.73.118:22,dr1stx@155.138.246.205:22,dr1stx@144.202.79.65:22,dr1stx@108.61.222.139:22 dory@10.1.1.50 -L 127.0.0.1:2222:127.0.0.1:7869
```

# KASM
Looks amazing!

https://www.kasmweb.com/docs/latest/install/single_server_install.html

https://youtu.be/U7e-mcJdZok


