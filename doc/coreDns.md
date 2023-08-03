### CoreDNS

쿠버네티스 클러스터의 DNS 역할을 수행하는 서버

기본적으로 알고 있는 DNS 서버와 차별점이 있다면 pod domain, service domain에 대한 DNS 응답을 한다.

* reference
    - https://dobby-isfree.tistory.com/202
    - https://jonnung.dev/kubernetes/2020/05/11/kubernetes-dns-about-coredns/#gsc.tab=0

* 조회
```
$ kubectl get all -A | grep dns
kube-system            pod/coredns-5d78c9869d-l2wkp                            1/1     Running     7 (5d3h ago)    19d
kube-system            pod/coredns-5d78c9869d-vckvs                            1/1     Running     7 (5d3h ago)    19d
kube-system            service/kube-dns                    ClusterIP      10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP   19d
kube-system            deployment.apps/coredns                     2/2     2            2           19d
kube-system            replicaset.apps/coredns-5d78c9869d                     2         2         2       19d

$ kubectl get po -n kube-system -l k8s-app=kube-dns
NAME                       READY   STATUS    RESTARTS       AGE
coredns-5d78c9869d-l2wkp   1/1     Running   7 (5d4h ago)   19d
coredns-5d78c9869d-vckvs   1/1     Running   7 (5d4h ago)   19d

$ kubectl get svc -n kube-system -l k8s-app=kube-dns
NAME       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   19d
```

* codeDns 정보
```
$ kubectl describe cm -n kube-system coredns
Name:         coredns
Namespace:    kube-system
Labels:       <none>
Annotations:  <none>

Data
====
Corefile:
----
.:53 {
    errors
    health {
       lameduck 5s
    }
    ready
    kubernetes cluster.local in-addr.arpa ip6.arpa {
       pods insecure
       fallthrough in-addr.arpa ip6.arpa
       ttl 30
    }
    prometheus :9153
    forward . /etc/resolv.conf {
       max_concurrent 1000
    }
    cache 30
    loop
    reload
    loadbalance
}


BinaryData
====

Events:  <none>
```