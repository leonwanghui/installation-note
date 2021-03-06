# OpenSDS Docker Install Tutorial

## Pre-tips

1. How to run a single node etcd cluster with endpoint configured?
```
etcd --advertise-client-urls http://100.64.128.40:2379 --listen-client-urls http://100.64.128.40:2379,http://127.0.0.1:2379
```

2. How to run a single node ceph cluster in docker for test?
```
sudo docker run -d --net=host -v /etc/ceph:/etc/ceph -e MON_IP=100.64.128.40 -e CEPH_PUBLIC_NETWORK=100.64.128.0/24 ceph/demo
```
If some errors occur in rbd map: ```YOUR_HOSTNAME 100.64.128.40:6789 feature set mismatch```, it can be solved in two ways:
- ```ceph osd crush tunables legacy```
- Add ```rbd_default_features = 1``` one line in ```/etc/ceph/ceph.conf``` file

## OpenSDS Service Installation

If you are a lazy one, just like me, you probably want to do this:
```
docker run -it --net=host -v /var/log/opensds:/var/log/opensds leonwanghui/opensds-controller:v1alpha1

docker run -d --net=host -v /var/log/opensds:/var/log/opensds -v /etc/opensds:/etc/opensds -v /etc/ceph:/etc/ceph leonwanghui/opensds-dock:v1alpha1
```

If you are a smart guy, you probably need to configure your service ip and database endpoint:
```
docker run -it --net=host -v /var/log/opensds:/var/log/opensds leonwanghui/opensds-controller:v1alpha1 /usr/bin/osdslet --api-endpoint=127.0.0.1:50040 --db-endpoint=127.0.0.1:2379,127.0.0.1:2380 --osdsletlog-file=/var/log/opensds/osdslet.log

docker run -d --net=host -v /var/log/opensds:/var/log/opensds -v /etc/opensds:/etc/opensds -v /etc/ceph:/etc/ceph leonwanghui/opensds-dock:v1alpha1 /usr/bin/osdsdock --api-endpoint=127.0.0.1:50050 --db-endpoint=127.0.0.1:2379,127.0.0.1:2380 --osdsletlog-file=/var/log/opensds/osdsdock.log
```

After this is done, just enjoy it!
