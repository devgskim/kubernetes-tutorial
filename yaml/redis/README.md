### 작업 대기열을 사용한 정밀 병렬 처리

* https://kubernetes.io/ko/docs/tasks/job/fine-parallel-processing-work-queue/


### Example: Deploying PHP Guestbook application with Redis

* https://kubernetes.io/docs/tutorials/stateless-application/guestbook/
~~~yaml
# SOURCE: https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-leader
  labels:
    app: redis
    role: leader
    tier: backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
        role: leader
        tier: backend
    spec:
      containers:
      - name: leader
        image: "docker.io/redis:6.0.5"
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
~~~
