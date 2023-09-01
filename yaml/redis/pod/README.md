### case1

* start service
~~~bash
$ kubectl apply -f redis-pod-504.yaml
$ kubectl get endpoints
$ kubectl get svc
~~~

* delete service
~~~bash
$ kubectl delete svc redis-service
$ kubectl delete configmap example-redis-config
$ kubectl delete pod redis
~~~

### case2

* start service
~~~bash
$ kubectl apply -f redis-pod.yaml
$ kubectl apply -f redis-pod-service.yaml
~~~

* start service
~~~bash
$ kubectl get endpoints
$ kubectl get svc
$ redis-cli -h 127.0.0.1 -p 6379 -a password
127.0.0.1:30379> ping
PONG
~~~

* delete service
~~~bash
$ kubectl delete svc redis-service
$ kubectl delete pod redis
~~~
