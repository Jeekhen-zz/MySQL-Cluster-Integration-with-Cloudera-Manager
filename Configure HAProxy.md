# Configuring HAProxy

https://www.digitalocean.com/community/tutorials/how-to-use-haproxy-to-set-up-mysql-load-balancing--3

https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Load_Balancer_Administration/s1-haproxy-setup-global.html

yum install haproxy
vi /etc/haproxy/haproxy.cfg

## Haproxy.cfg
```
listen mysql-cluster
    bind *:3306
    mode tcp
    balance roundrobin
    server mysql1 mysql1:3306 check
    server mysql2 mysql2:3306 check
```