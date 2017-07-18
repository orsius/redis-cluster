# Redis Cluster (2 nodes) - monitor by sentinel (3 nodes)
The following redis cluster configuration is the following: 3 Different linux servers

* srv 1 => redis master|(slave) + sentinel 1
* srv 2 => redis (master)|slave + sentinel 2
* srv 3 => sentinel 3 (sentinel only to avoid split brain situation)

## the redis version
```
$ redis_version:3.2.3
$ redis_mode:sentinel
$ os:Linux 3.10.0-514.21.2.el7.x86_64 x86_64
$ tcp_port:26379 # sentinel
$ tcp_port:6379  # redis```


## Requirements
The prerequisits are simples:
* have 3 linux servers ready to go with network already configure (ip, firewall, selinux, ntpd)
* have already install redis packages (redis server and sentinel)

## Running
* Clone the repository.
* Modify the bind ip address and all other ip references according to the server your are working on.
* start redis service (two nodes) then the redis sentinel service (three nodes)
And you are redis to go ;)

### Centos7
Run the following command.

```sh
# install packages
$ curl -L -O https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && yum install epel-release-latest-7.noarch.rpm
$ yum repolist && yum install redis

# start services
$ systemctl start redis.service && systemctl status redis.service
$ systemctl start redis-sentinel.service && systemctl status redis-sentinel.service

# connect to redis and check your cluster state
$ redis-cli -h ${SERVER_NODE} -p 6379 -a ${YOUR_PASSWORD} ping
$ redis-cli -h ${SERVER_NODE} -p 26379 sentinel ckquorum redis-cluster
```

Enjoy ;)

## information
More information can be found on redis website
* [redis.io - cluster ref ](https://redis.io/topics/cluster-tutorial)

## Optional
If your server is not a fresh install you could already create a redis user:group on all machines just to align the uid and gid values
`groupadd -g 2005 redis
useradd -u 2005 -s "/sbin/nologin" -c "redis user" -g 2005 redis`
To create a password you can use the following:
`date | md5sum`
