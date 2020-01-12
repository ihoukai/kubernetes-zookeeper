# Zookeeper on Kubernetes

simplify the creation and operation of zookeeper deployment in Kubernetes.

## How it works

When you create the resources in Kubernetes, it will create a 3-member (the minimum recommended size) [Stateful Set](https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/) cluster where the one member become leader and other members are follower.

## Testing it out

To launch the cluster, have Kubernetes create all the resources in zookeeper.yaml:

```
$ kubectl create -f redis-cluster.yaml
configmap/zookeeper-sh created
configmap/zookeeper-cfg created
service/zk-hs created
service/zk-cs created
poddisruptionbudget.policy/zk-pdb created
statefulset.apps/zk created
```

Wait a bit for the service to initialize.

Once all the pods are initialized, you can see pod list.

```
$ kubectl get pod
NAME   READY   STATUS    RESTARTS   AGE
zk-0   1/1     Running   0          21s
zk-1   1/1     Running   0          21s
zk-2   1/1     Running   0          21s
```

```
$ for i in 0 1 2; do echo zk-$i; kubectl exec zk-$i -- sh -c 'echo stat|nc 127.0.0.1 2181'; done;
zk-0
Zookeeper version: 3.5.6-c11b7e26bc554b8523dc929761dd28808913f091, built on 10/08/2019 20:18 GMT
Clients:
 /127.0.0.1:58382[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 29
Sent: 28
Connections: 1
Outstanding: 0
Zxid: 0x0
Mode: follower
Node count: 5
zk-1
Zookeeper version: 3.5.6-c11b7e26bc554b8523dc929761dd28808913f091, built on 10/08/2019 20:18 GMT
Clients:
 /127.0.0.1:54246[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 27
Sent: 26
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: follower
Node count: 5
zk-2
Zookeeper version: 3.5.6-c11b7e26bc554b8523dc929761dd28808913f091, built on 10/08/2019 20:18 GMT
Clients:
 /127.0.0.1:58386[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/0
Received: 29
Sent: 28
Connections: 1
Outstanding: 0
Zxid: 0x100000000
Mode: leader
Node count: 5
Proposal sizes last/min/max: -1/-1/-1
```
