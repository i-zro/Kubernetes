

---

# Taint, Toleration, Node Affinity 구분

## Taint, Toleration
각 노드에 Taint(오염)을 설정해서 감염으로부터 Toleration(용인)된 pod만 해당 노드에서 실행시켜준다.

![image](https://user-images.githubusercontent.com/48379869/232633236-f3eccc64-276b-4028-8539-63cb7a8b6e6c.png)

Pod 1은 red와 blue taint에 대해 toleration이 있기 때문에 Node1, Node2에 실행될 수 있고

Pod 2는 허용된 toleration이 없기 때문에 3 곳 모두에서 실행될 없고

Pod 3은 green toleration이 있기 때문에 Node 3에서 실행될 수 있다.

- taint 설정 시에는 kubectl taint nodes {NODE_NAME} key=value:{TAINT_EFFECT}, 해제 시에는 kubectl taint nodes {NODE_NAME} key=value:{TAINT_EFFECT}-

## Node Affinity

- pod가 특정 노드에만 배포되게 하는 것 

ex) `disktype: ssd`라는 requiredDuringSchedulingIgnoredDuringExecution 노드 어피니티를 가진 파드 → 파드가 `disktype=ssd` 레이블이 있는 노드에만 스케줄될 것

## Taint와 Affinity 차이

taint가 특정노드에 pod가 배포되지 못하도록 하는 정책이라면, affinity는 pod를 특정 노드에만 배포되도록 하는 정책

---

# (미해결) 왜 pending state인지 어떻게 알지? (Manual Scheduling)

`230417`

- Why is the POD in a pending state? Inspect the environment for various kubernetes control plane components.

해답이

Run the command: kubectl get pods --namespace kube-system to see the status of scheduler pod. We have removed the scheduler from this Kubernetes cluster. As a result, as it stands, the pod will remain in a pending state forever.

위와 같은데, scheduler라는 pod가 없어서 scheduler가 없다는 건지 진짜 모르겠음.

뭐 어캐 찾는겨?

<img width="637" alt="12" src="https://user-images.githubusercontent.com/48379869/232472693-9d2a62b2-82e6-4092-9f33-27ad63b000e6.png">

---

# (미해결) 언제 run쓰고 언제 expose 써야할 지 모르겠음. (Imperative Commands)

`230417`

- Create a pod called `httpd` using the image `httpd:alpine` in the default namespace. Next, create a service of type `ClusterIP` by the same name `(httpd)`. The target port for the service should be `80`. Try to do this with as few steps as possible.

해당 문제의 정답이 kubectl run httpd --image=httpd:alpine --port 80 --expose 이건데 뭔가 expose 와 clusterip에 꽂혀서 헤매고 있었음.

둘이 어떻게 구분하는지? 생각회로가 궁금함.

---

# (미해결) yaml 파일 label 구조 (ReplicaSets)

`230416`

정확히는 replicaset 관련한 의문은 아니긴한데, yaml 파일 구조에 대한 지식 부족에 의해 나오는 의문 같다.

뭔가 Kodekloud 랩에서 아래와 같이 고쳤는데 넘어가지긴 했는데 (원래 template > metadata > labels> tier가 nginx라고 되어있었을텐데 docs 보면서 frontend로 고쳤던 것 같다.)

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

실제 kodekloud에서 제시한 정답이 아래와 같았는데 아직 저게 어떻게 엮여서 replicaset이 생성되는 구조인 건지 모르겠는데 일단 한 번 돌리고 공부해보려 한다.

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-2
spec:
  replicas: 2
  selector:
    matchLabels:
      tier: nginx
  template:
    metadata:
      labels:
        tier: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```
