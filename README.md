# Kubernetes Test

a simple playground for playing around with kubernetes.
Testing with minikube and kubectl on one machine.

## Requirements
a machine with at least 2 cores and 2GB RAM. And docker or any other supported container system. Here I am using docker.

## Start minikube


```bash
minikube start --driver docker
# check if all is running
minikube status
```


## Working with pods

### start everything

```bash
kubectl apply -f mongo-config.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml
kubectl apply -f webapp.yaml
```


### check the status

```bash
kubectl get all
```

output:

```
NAME                                     READY   STATUS    RESTARTS   AGE
pod/mongo-deployment-564b4bdfdf-gt46p    1/1     Running   0          5m16s
pod/webapp-deployment-56dbf5d695-442sz   1/1     Running   0          4m26s

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes       ClusterIP   10.96.0.1       <none>        443/TCP          49m
service/mongo-service    ClusterIP   10.104.152.87   <none>        27017/TCP        5m16s
service/webapp-service   NodePort    10.109.83.118   <none>        3000:30100/TCP   4m26s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongo-deployment    1/1     1            1           5m16s
deployment.apps/webapp-deployment   1/1     1            1           4m26s

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/mongo-deployment-564b4bdfdf    1         1         1       5m16s
replicaset.apps/webapp-deployment-56dbf5d695   1         1         1       4m26s
```

### check secrets

```bash
kubectl get secret
```

output:

```
NAME                  TYPE                                  DATA   AGE
default-token-p4bqj   kubernetes.io/service-account-token   3      51m
mongo-secret          Opaque                                2      8m22s
```

### get details for a service

here as example for service "webapp-service"
(all running services are visible for example in `kubectl get all`)

```bash
kubectl describe service webapp-service
```

the describe param can also show many other infos, eg about the pod:

```bash
kubectl describe pod some-pod-id
```

### get the logs of a pod

```bash
kubectl logs some-pod-id
```
this will return the last log entries. To get a live stream of logs, append `-f`  th that command.

### get the IP

to get the public ip for the container by using minikube:

```bash
minikube ip
```

or with cubectl:

```bash
kubectl get note -o wide
```

output:

```
NAME       STATUS   ROLES                  AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION    CONTAINER-RUNTIME
minikube   Ready    control-plane,master   63m   v1.23.3   192.168.49.2   <none>        Ubuntu 20.04.2 LTS   5.10.0-11-amd64   docker://20.10.12
```

