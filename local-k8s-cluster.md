# Local k8s Cluster Installation

## Pre-configuration

### Download and install Golang

```
wget https://storage.googleapis.com/golang/go1.7.6.linux-amd64.tar.gz
tar xvf go1.7.6.linux-amd64.tar.gz -C /usr/local/
mkdir -p $HOME/gopath/src
mkdir -p $HOME/gopath/bin
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/gopath/bin' >> /etc/profile
echo 'export GOPATH=$HOME/gopath' >> /etc/profile
source /etc/profile
go version
```

### Install some kernel dependencies

```
apt-get install gcc make libc-dev docker.io
```
if the docker command doesn't work, try to restart it:
```
sudo service docker stop
sudo nohup docker daemon -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock &
```

## Download and unpackage k8s and etcd source code

```
wget https://github.com/kubernetes/kubernetes/archive/v1.6.0.tar.gz
wget https://github.com/coreos/etcd/releases/download/v3.2.0/etcd-v3.2.0-linux-amd64.tar.gz
tar -zxvf etcd-v3.2.0-linux-amd64.tar.gz
cp etcd-v3.2.0-linux-amd64/etcd /usr/local/bin
tar -zxvf v1.6.0.tar.gz
```

## Run your own cluster

```
go get -u github.com/cloudflare/cfssl/cmd/...
kubernetes-1.6.0/hack/local-up-cluster.sh
```
if you want to run it background, then run:
```
nohup kubernetes-1.6.0/hack/local_up_cluster.sh &>> kube.log &
```
if you want to set up an environment for service catalog, then run:
```
KUBE_ENABLE_CLUSTER_DNS=true nohup kubernetes-1.6.0/hack/local-up-cluster.sh -O &>> kube.log &
```
