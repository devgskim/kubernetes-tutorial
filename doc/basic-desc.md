# 쿠버네티스 기본

* site: [kubernetes](https://kubernetes.io/ko/)


* namespace & api
```bash
$ kubectl get namespace
$ kubectl config view
$ kubectl config view --minify | grep namespace  # on linux
$ kubectl config set-context --current --namespace={change-ns}

$ kubectl api-resources
```

* pod
```bash
$ kubectl get pod --show-labels
$ kubectl get pod -l {label}
$ kubectl get pod -l {label} --show-labels
$ kubectl get pod -L {label key} --show-labels

$ kubectl create -f {pod.yaml}  # 최초 생성시만. 
$ kubectl apply -f {pod.yaml}
$ kubectl apply -f {pod.yaml} -n {namespace}
$ kubectl delete pod {pod name}
$ kubectl describe pod {pod name}
$ kubectl exec {pod name} env
```

* pod
```bash
$ kubectl get statefulset {statefulset name}
```

* pv & pvc
```bash
$ kubectl get pv --show-labels
$ kubectl get pvc --show-labels
```

* service
```bash
$ kubectl get svc --show-labels
```

* replicaSet
```bash
$ kubectl get rs
$ kubectl delete rs {replicaSet name}
$ kubectl scale rs {replicaSet name} --replicas=0
$ kubectl edit rs {replicaSet name}
```

* deploy
```bash
$ kubectl get deploy
$ kubectl scale deploy/{deploy name} --replicas=0
$ kubectl delete deploy {deploy name}
$ kubectl describe deploy {deploy name}
```


<details><summary>kubectl log</summary>
<p>

###### 로그 확인

```bash
$ kubectl logs pod-name
$ kubectl logs pod-name --since=2h
$ kubectl logs pod-name --tail=10
$ kubectl logs my-pod > my-pod-logs.txt
$ kubectl logs -f pod-name
```

</p>
</details>