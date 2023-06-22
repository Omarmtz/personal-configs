### UNDER CONSTRUCTION 

Certificates :

>OPENSSL
>Is the tool that is used to generate our own certificate

1. We need to generate a key pair 
> Public and private keys

Fullchain certificate file?

- client cert and the output of the CA.cert 

48:21:0b:51:12:48
172.16.0.5

```
openssl genrsa -aes256 -out ca-key.pem 4096
Generating RSA private key, 4096 bit long modulus
......................................++++
...............................++++
e is 65537 (0x10001)
Enter pass phrase for ca-key.pem:
Verifying - Enter pass phrase for ca-key.pem:
```


1. Create CA Key Secret
2. Create Certificate for the CA
3. Create Client and subdomains... 
	1. For target address 
4


PROXMOX TRUST OUR CA : 
```bash
scp ./CA/certificate_authority.pem root@172.16.2.5:/usr/local/share/ca-certificates/ca.crt
root@172.16.2.5's password:
certificate_authority.pem                                                                100% 2021   404.7KB/s   00:00
```

and then 

```bash
update-ca-certificates
```


```bash
root@influxdb:~/config# cat /usr/lib/influxdb/scripts/influxd-systemd-start.sh
#!/bin/bash -e

/usr/bin/influxd &
PID=$!
echo $PID > /var/lib/influxdb/influxd.pid

PROTOCOL="https"
BIND_ADDRESS=$(influxd print-config --key-name http-bind-address)
TLS_CERT=$(influxd print-config --key-name tls-cert | tr -d '"')
TLS_KEY=$(influxd print-config --key-name tls-key | tr -d '"')
if [ -n "${TLS_CERT}" ] && [ -n "${TLS_KEY}" ]; then
  echo "TLS cert and key found -- using https"
  PROTOCOL="https"
fi
HOST=${BIND_ADDRESS%:*}
HOST=${HOST:-"localhost"}
PORT=${BIND_ADDRESS##*:}

set +e
attempts=0
url="$PROTOCOL://$HOST:$PORT/ready"
result=$(curl -k -s -o /dev/null $url -w %{http_code})
while [ "${result:0:2}" != "20" ] && [ "${result:0:2}" != "40" ]; do
  attempts=$(($attempts+1))
  echo "InfluxDB API at $url unavailable after $attempts attempts..."
  sleep 1
  result=$(curl -k -s -o /dev/null $url -w %{http_code})
done
echo "InfluxDB started"
set -e
```