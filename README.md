# Kubernetes (a.k.a. k8s)

## Terms
### 1. Object
k8s가 리소스를 처리하는 단위로, Pods, Replica set, Node도 모두 Object에 포함된다.
~~~shell
$ kubectl api-resources
~~~
위 명령어를 통해 k8s에서 사용할 수 있는 오브젝트 종류를 확인할 수 있다.

<br/>

### 2. Pod
k8s에서 컨테이너 애플리케이션의 기본 단위로, 1개 이상의 컨테이너로 구성된 컨테이너의 집합이다.

주로 하나의 파드에 하나의 컨테이너로 구성된 파드를 많이 사용하지만, 부가 기능이 필요한 경우 Sidecar container를 파드에 포함시키기도 한다.

*[이해 안 됨/ p.290~p.293] Linux namespace를 공유하는 여러 컨테이너들을 추상화하여 사용하기 위해 파드를 사용한다.*

<br/>

### 3. Replica Set
노드 장애 등 파드의 상태를 모니터링하고 일정한 개수의 파드를 유지시켜주는 오브젝트이다.

Pod에는 monitoring 혹은 managing 기능이 존재하지 않으므로 마이크로 서비스가 어렵기에, Lifecycle을 관리할 수 있도록 Replica set을 사용한다.

<br/>

### 4. Deployment
Replica set의 상위 object로, Replica set과 Pod의 정보를 정의한다.

애플리케이션의 업데이트와 배포를 편리하게 만들기 위해 주로 사용하며, Rollback, Rolling update 등을 지원한다.

<br/>

### 5. Service
여러 개의 deployment를 하나의 애플리케이션으로 연결하고, 외부에서 접근할 수 있도록 여러 기능을 제공하는 오브젝트이다.

크게 Cluster IP, NodePort, LoadBalancer 3가지 타입으로 나뉘어진다.
* Cluster IP: Kubernetes 내부에서만 파드에 접근할 때 사용
* NodePort: 외부에서 파드에 접근할 수 있도록 포트를 개방할 때 사용
* LoadBalancer: 클라우드 플래폼 환경에서만 사용

그러나 NodePort 서비스를 외부에 제공하기 보다는 Ingress object를 이용하여 SSL, Routing 등 설정하여 간접적으로 사용한다.

<br/>

### 6. Namespace
용도에 따라 컨테이너 및 관련 리소스를 구분 지어 관리할 수 있도록 일종의 논리적인 그룹을 제공하는 오브젝트이다.

특정 네임 스페이스에서만 특정 목적의 object들이 존재하도록 하며, 주로 Monitoring, Load balancing, Ingressing 등의 목적으로 사용한다.

Linux namespace와는 다른 개념이므로 혼동하지 않도록 주의한다.






<br/><br/>
## How to use k8s
### 1. Create the object (pod, replica set, ...) based yaml file
```shell
$ kubectl apply -f `file_name`.yaml
```
### 2. Check detailed information of created object
```shell
$ kubectl describe (object to show: pods, deploy, ...) `file_name`
```
### 3. Ececute container
아래 명령어를 수행하면, 파드에 포함된 컨테이너 내부의 bash shell을 실행하되 -it 옵션으로 셸을 유지한다.
```shell
$ kubecttl exec -it `file_name` bash
```
### 4. Check log
```shell
$ kubectl logs `file_name`
```
### 5. Delete the object
```shell
$ kubectl delete rs `file_name`
```
### 6. Clear resources
```shell
$ kubectl delete (objects to delete: deployment, pod, rs, ...) --all
```




## Kubernettes vs. Docker swarm
* **[추가 조사 필요]** 단일 Docker daemon으로 구성된 docker swarm과 달리, k8s는 다양한 컨테이너 기반의 컴포넌트로 구성되어 있다.
  * Docker engine -> 기본 단위: Docker container
  * Docker swarm -> 기본 단위: Service (여러 Docker contanier들의 집합)
  * k8s -> 기본 단위: Pod 


## References
1. Kubernetes official website: https://kubernetes.io/
2. 시작하세요! 도커/쿠버네티스: https://m.yes24.com/Goods/Detail/84927385
