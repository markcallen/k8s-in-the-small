# K8S in the Small

Running kubernetes with just a nfew nodes

## Pod Affinity

from https://stackoverflow.com/questions/41159843/kubernetes-pod-distribution-amongst-nodes


### Setup
create a namespace

```
kubectl create ns say-app
```

### Run an app

This will startup 6 pods

```
kubectl apply -f say.yaml -n say-app
kubectl get pods -n say-app -o wide
```

```
NAME                              READY   STATUS    RESTARTS   AGE   IP             NODE                                         NOMINATED NODE   READINESS GATES
say-deployment-5f74658f75-8xk6d   1/1     Running   0          14s   10.112.1.132   ip-10-112-1-18.us-west-2.compute.internal    <none>           <none>
say-deployment-5f74658f75-hkdqp   1/1     Running   0          14s   10.112.1.56    ip-10-112-1-216.us-west-2.compute.internal   <none>           <none>
say-deployment-5f74658f75-m8hlf   1/1     Running   0          14s   10.112.2.170   ip-10-112-2-131.us-west-2.compute.internal   <none>           <none>
say-deployment-5f74658f75-wnmjq   1/1     Running   0          14s   10.112.2.228   ip-10-112-2-163.us-west-2.compute.internal   <none>           <none>
say-deployment-5f74658f75-wx2vz   1/1     Running   0          14s   10.112.1.219   ip-10-112-1-13.us-west-2.compute.internal    <none>           <none>
say-deployment-5f74658f75-zkmv2   1/1     Running   0          14s   10.112.2.208   ip-10-112-2-70.us-west-2.compute.internal    <none>           <none>
```

Now it is possible for these pods to be all on 1 server, so lets add podAntiAffinity


```
kubectl apply -f say-only-one.yaml -n say-app
kubectl get pods -n say-app -o wide
```


Now lets make sure that all the pods run, even doubling up where necessary.


```
kubectl apply -f say-at-least-one.yaml -n say-app
kubectl get pods -n say-app -o wide
```


Teardown

kubectl delete ns say-app
