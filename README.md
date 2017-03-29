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
yum remove mariadb-libs

Download MySQL-Cluster from the web
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-management-server-7.5.5-1.el7.x86_64.rpm
wget 192.168.52.142/mysql_cluster/mysql-cluster-community-data-node-7.5.5-1.el7.x86_64.rpm



yum install mysql-cluster-community-common-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-libs-7.5.5-1.el7.x86_64.rpm

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

ndb_mgmd -f mycluster/config.ini --initial
ndb_mgm
ndb_mgm> show

ndbd

systemctl start mysqld



yum remove mysql-cluster-community-client.x86_64 mysql-cluster-community-server.x86_64
yum install mysql-cluster-community-client-7.5.5-1.el7.x86_64.rpm mysql-cluster-community-server-7.5.5-1.el7.x86_64.rpm
```

## my.cnf
```
[mysqld]
ndbcluster
default-storage-engine=NDBCLUSTER

[mysql_cluster]
ndb-connectstring=192.168.52.140

```

## mycluster/config.ini
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

## Common Problem
```
Can't open the mysql.plugin table.
```

