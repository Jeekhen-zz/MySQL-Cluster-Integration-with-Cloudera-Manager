# MySQL-Cluster-Integration-with-Cloudera-Manager

```
* Require EPEL for rhel
http://download.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm   

* Remember to off Firewalld

* Can run 'netstat ntlp' to check ports

Things you need to install:
(ServerNode)
Perl-Data-Dumper
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



Commands are as below:
yum remove mariadb-libs

Download MySQL-Cluster from the web
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-management-server-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-data-node-7.5.5-1.el7.x86_64.rpm



yum install mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm 
yum install mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm

For Server nodes:
yum install mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm
yum install mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm

For Data Nodes:
yum install mysql-cluster-community-data-node-7.5.5-1.el7.x86_64.rpm

For Mgt Nodes:
yum install mysql-cluster-community-management-server-7.5.5-1.el7.x86_64.rpm

Create/Modify 2 configuration file (Refer to below):
1) /root/mycluster/config.ini (This is for your ndb manager only)
2) /etc/my.cnf (this are for your data nodes)

cd /usr/bin
ndb-mgmd -f mycluster/config.ini 



```

** # my.cnf **
```
[mysqld]
ndbcluster
default-storage-engine=NDBCLUSTER

[mysql_cluster]
ndb-connectstring=192.168.52.140

```

** # mycluster/config.ini **
```
[ndb_mgmd]
hostname=192.168.52.140
datadir=/root/mycluster

[ndbd default]
NoOfReplicas=2
DataMemory=100M
IndexMemory=10M
ServerPort=2202

[ndbd]
hostname=192.168.52.140
datadir=/root/mycluster-data

[ndbd]
hostname=192.168.52.141
datadir=/root/mycluster-data2

[mysqld]
hostname=192.168.52.140

[mysqld]
hostname=192.168.52.141
```