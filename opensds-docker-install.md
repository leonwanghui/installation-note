# OpenSDS Docker Build and Install

## Pre-tips

1. How to run a single node etcd cluster with endpoint configured?
```
etcd --advertise-client-urls http://100.64.128.40:2379 --listen-client-urls http://100.64.128.40:2379,http://127.0.0.1:2379
```

2. How to run a single node ceph cluster in docker for test?
```
sudo docker run -d --net=host -v /etc/ceph:/etc/ceph -e MON_IP=100.64.128.40 -e CEPH_PUBLIC_NETWORK=100.64.128.0/24 ceph/demo
```
