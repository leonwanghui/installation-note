# OpenSDS work with Kubernetes Service Catalog Tutorial

## Set up minikube environment (Virtualbox prepared)

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.7.0/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl

minikube start

kubectl get po (check if the cluster is up)
```

## Install some dependencies

1. Helm (from script)

```
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh

kubectl get po -n kube-system (check if the till-deploy pod is running)
```

2. Service Catalog

```
git clone https://github.com/kubernetes-incubator/service-catalog.git

cd gopath/src/github.com/kubernetes-incubator/service-catalog
helm install charts/catalog --name catalog --namespace catalog

kubectl get po -n catalog (check if the catalog api-server and controller-manager pod are running)
```

3. OpenSDS Broker

```
git clone https:// github.com/leonwanghui/opensds-broker.git

cd gopath/src/github.com/leonwanghui/opensds-broker
helm install charts/opensds-broker --name opensds-broker --namespace opensds-broker

kubectl get po -n opensds-broker (check if opensds broker pod is running)
```

## Configure Kubectl context

```
kubectl config set-cluster service-catalog --server=http://$SVC_CAT_API_SERVER_IP:30080
kubectl config set-context service-catalog --cluster=service-catalog

kubectl context=service-catalog get brokers, instances, bindings
```

## Start to work

1. Create opensds broker

```
kubectl --context=service-catalog create -f examples/opensds-broker.yaml

kubectl --context=service-catalog get brokers, serviceclasses
```

2. Create opensds instance

```
kubectl --context=service-catalog create -f examples/opensds-instance.yaml -n opensds

kubectl --context=service-catalog get instances -n opensds
```

3. Create opensds instance binding

```
kubectl --context=service-catalog create -f examples/opensds-binding.yaml -n opensds

kubectl --context=service-catalog get bindings -n opensds
kubectl get secrets -n opensds
```

## Clear it up

1. Delete opensds instance binding

```
kubectl --context=service-catalog delete -f examples/opensds-binding.yaml -n opensds
```

2. Delete opensds instance

```
kubectl --context=service-catalog delete -f examples/opensds-instance.yaml -n opensds
```

3. Delete opensds broker

```
kubectl --context=service-catalog delete -f examples/opensds-broker.yaml
```

4. Uninstall opensds broker pod

```
helm delete --purge opensds-broker
```

5. Uninstall service catalog pods

```
helm delete --purge catalog
```
