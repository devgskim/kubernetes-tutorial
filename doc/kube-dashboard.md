## 쿠버네티스 Dashboard 

* [대시보드 가이드](https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/)

```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.6.1/aio/deploy/recommended.yaml
```

* [샘플 사용자 가이드 만들기](https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md)
    - kube-dashboard.yaml

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard

---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard

```

* 사용자 및 토큰 생성
```
$ apply -f kube-dashboard.yaml
serviceaccount/admin-user created
clusterrolebinding.rbac.authorization.k8s.io/admin-user created

$ kubectl -n kubernetes-dashboard create token admin-user
eyJhbGciOiJSUzI1NiIsImtpZCI6ImN4SXotM....
```

* proxy 실행
```
$ kubectl proxy 
Starting to serve on 127.0.0.1:8001
```

* 웹 대시보드 접속
    - 토큰값 입력
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

* 사용자 삭제
```
kubectl -n kubernetes-dashboard delete serviceaccount admin-user
kubectl -n kubernetes-dashboard delete clusterrolebinding admin-user
```
