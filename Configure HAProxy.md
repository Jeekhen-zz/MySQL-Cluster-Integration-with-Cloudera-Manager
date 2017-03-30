# Configuring HAProxy

https://www.digitalocean.com/community/tutorials/how-to-use-haproxy-to-set-up-mysql-load-balancing--3


## Haproxy.cfg
```
global
    log 127.0.0.1 local0 notice
    user root
    group root
	
defaults
    log global
    retries 2
    timeout connect 3000
    timeout server 5000
    timeout client 5000
	
	
listen mysql-cluster
    bind 127.0.0.1:3306
    mode tcp
    option mysql-check user haproxy_check
    balance roundrobin
    server mysql1 mysql1:3306 check
    server mysql2 mysql2:3306 check
	
listen 0.0.0.0:8080
    mode http
    stats enable
    stats uri /
    stats realm Strictly\ Private
    stats auth A_Username:YourPassword
    stats auth Another_User:passwd

	
```