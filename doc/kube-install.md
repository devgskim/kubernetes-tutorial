## 설치단계

* 설치
    1. [쿠버네티스 다운로드](https://kubernetes.io/releases/download/)
    1. [kubectl 설치](https://kubernetes.io/ko/docs/tasks/tools/install-kubectl-linux/)

* 쿠버네티스 컨테이너 이미지

| 컨테이너 | 버전 | Supported Architectures |
|:--- |:--- |:--- | 
|registry.k8s.io/kube-apiserver         | v1.27.4 | amd64, arm, arm64, ppc64le, s390x |
|registry.k8s.io/kube-controller-manager| v1.27.4 | amd64, arm, arm64, ppc64le, s390x |
|registry.k8s.io/kube-proxy             | v1.27.4 | amd64, arm, arm64, ppc64le, s390x |
|registry.k8s.io/kube-scheduler         | v1.27.4 | amd64, arm, arm64, ppc64le, s390x |
|registry.k8s.io/conformance            | v1.27.4 | amd64, arm, arm64, ppc64le, s390x |



```mermaid
---
title: 쿠버네티스 설치
---
flowchart LR

 kube[쿠버네티스 다운로드] --> kubectl[kubectl 도구설치]

```