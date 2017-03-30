# MySQL-Cluster-Integration-with-Cloudera-Manager

```
* Require EPEL for rhel
http://download.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm   
* Remember to off Firewalld and SELinux
(If SELinux not turn off. mysqld will have problem starting.)
* Can run 'netstat ntlp' to check ports if ports are opened
* Remember to turn off your computer Firewall if you are testing on VM on your own machine

Things you need to install:
(ServerNode)
Shared Libs
Cluster-Server

(DataNode)
Perl-Data-Dumper
Shared Libs
Cluster-Server

(Management Node)
Perl-Data-Dumper
Shared Libs
Cluster-Server

Setup yum repository for RHEL CD and RHEL EPEL package. Google has plenty of instruction to do this.

Commands are as below:
sudo yum remove mariadb-libs

Download MySQL-Cluster from the web
wget repo.local/mysql_cluster/mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm
wget repo.local/mysql_cluster/mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm
wget repo.local/mysql_cluster/mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm
wget repo.local/mysql_cluster/mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm
wget repo.local/mysql_cluster/mysql-cluster-community-management-server-7.5.5-1.el7.x86_64.rpm
wget repo.local/mysql_cluster/mysql-cluster-community-data-node-7.5.5-1.el7.x86_64.rpm
* repo.local is my hostname for my VM hosting the repo


sudo yum install mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm

For Server nodes:
sudo yum install mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm
sudo yum install mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm

For Data Nodes:
sudo yum install mysql-cluster-community-data-node-7.5.5-1.el7.x86_64.rpm
mkdir mycluster-data

For Mgt Nodes:
sudo yum install mysql-cluster-community-management-server-7.5.5-1.el7.x86_64.rpm
mkdir mycluster

Create/Modify 2 configuration file (Refer to below):
1) /root/mycluster/config.ini (This is for your ndb manager only)
2) /etc/my.cnf (this are for your data nodes)

ndb_mgmd -f mycluster/config.ini --initial
ndb_mgm
ndb_mgm> show

ndbd

systemctl start mysqld

cat /var/log/mysqld.log | grep password
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';

```

## Testing 
```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'MyNewPass4!' WITH GRANT OPTION;
**** (My sure to run grant privileges to all server(mysqld) nodes)

CREATE DATABASE cluster;
USE cluster;
CREATE TABLE cluster_test (name VARCHAR(20), value VARCHAR(20)) ENGINE=ndbcluster;
INSERT INTO cluster_test (name,value) VALUES('some_name','some_value');
SELECT * FROM cluster_test;
```


## my.cnf
```
[mysqld]
ndbcluster
default-storage-engine=NDBCLUSTER
log-error=/var/log/mysqld.log

[mysql_cluster]
ndb-connectstring=mysqlmgmt
```

## mycluster/config.ini
```
[ndb_mgmd]
hostname=mysqlmgmt.local
datadir=/root/mycluster

[ndbd default]
NoOfReplicas=2
DataMemory=100M
IndexMemory=10M
ServerPort=2202

[ndbd]
hostname=mysql1.local
datadir=/root/mycluster-data

[ndbd]
hostname=mysql2.local
datadir=/root/mycluster-data

[mysqld]
hostname=mysql1.local

[mysqld]
hostname=mysql2.local
```

## Common Problem
```
Can't open the mysql.plugin table.
```

## Pre OS setup
```
systemctl stop firewalld
systemctl disable firewalld
vi /etc/selinux/config 
```

## To remove mysql Cluster cleanly
```
yum list | grep mysql-cluster
yum remove <accordingly>

mysql-cluster-community-client.x86_64
mysql-cluster-community-common.x86_64
mysql-cluster-community-data-node.x86_64
mysql-cluster-community-libs.x86_64
mysql-cluster-community-server.x86_64

rm -rf /var/lib/mysql
rm -rf mycluster-data/
mkdir mycluster-data
find / -name mysql
```

## For personal use only. 
```
yum remove mysql-cluster-community-client.x86_64 mysql-cluster-community-common.x86_64 mysql-cluster-community-data-node.x86_64 mysql-cluster-community-libs.x86_64 mysql-cluster-community-server.x86_64

yum install mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-data-node-7.5.5-1.el7.x86_64.rpm

```