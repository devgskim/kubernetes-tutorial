
* reference: 
  - https://www.sumologic.com/blog/kubernetes-deploy-postgres/
  - https://arisu1000.tistory.com/27844
  - https://www.cloudytuts.com/guides/kubernetes/how-to-deploy-postgress-kubernetes/
  - https://www.crunchydata.com/blog/creating-a-postgresql-cluster-with-kubernetes-crds
  - https://sarc.io/index.php/cloud/2284-kubernetes-eks-postgresql-feat-ebs
 
* text base64
```
$ echo "password" | base64
cGFzc3dvcmQK
$ echo cGFzc3dvcmQK | base64 --decode
password
```

* file
```bash
$ echo hello > 1.txt
$ base64 1.txt
aGVsbG8K

$ echo aGVsbG8K > 2.txt
$ base64 -di 2.txt
hello
```

* secrets
```bash
$ echo -n 'username' > ./username.txt
$ echo -n 'password' > ./password.txt
$ kubectl create secret generic user-pass-secret --from-file=./username.txt --from-file=./password.txt

$ kubectl get secret user-pass-secret -o yaml
```

```bash
$ kubectl create secret generic postgres --from-literal=POSTGRES_USER="root" --from-literal=POSTGRES_PASSWORD="password"
```
