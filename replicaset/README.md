### ReplicaSet 실습

1. 레프리카세트 배포
```
kubectl apply -f simple-replicaset-yaml
```

2. 생성된 파드 확인
```
kubectl get pod
```

3. 레프리카세트 삭제
```
kubectl delete -f simple-replicaset.yaml
```
