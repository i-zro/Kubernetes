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
