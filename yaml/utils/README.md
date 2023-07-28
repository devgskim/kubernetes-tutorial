
* pod-exam-count.yaml

```bash
$ kubectl apply -f pod-exam-count.yaml
$ kubectl get pod
$ kubectl logs pod-exam-count
$ kubectl delete pod pod-exam-count
```

* cronjob
    - [설명](https://kubernetes.io/ko/docs/tasks/job/automated-tasks-with-cron-jobs/)
```
kubectl create -f https://k8s.io/examples/application/job/cronjob.yaml
```