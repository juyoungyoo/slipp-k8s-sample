### Deployment 실습
먼저 pod 3개를 생성한다.     deployment 설정을 변경하여 pod를 4개로 scale up한다.

1. 옵션으로 기록을 남기며 클러스터에 반영
```
kubectl apply -f simple-deployment.yaml --record
```
2. 상태 확인
```
kubectl get pod,replicaset,deployment --selector app=echo
```

3. 리비전 확인
```
kubectl rollout history deployment echo
```

4. 동일한 spec이며 파드 개수 만 수정한다.
기존 예제 코드에서 pod 복제 수를 늘린다.   
이 후 수정된 매니페스트 적용한다.   
scale up되었는지 확인한다.   
```
# 디플로이먼트 배포
kubectl apply -f simple-deployment-scale-up.yaml --record

# 리비전 확인
kubectl rollout history deployment echo
```

5. 컨테이너 정의 수정 시 리비전 변경이 있는지 확인한다.
(simple-deployment-scale-up.yaml)
```
# 디플로이먼트 배포
kubectl apply -f simple-deployment-update-spec.yaml --record

# 리비전 확인
kubectl rollout history deployment echo
```

6. 이전 리비전으로 롤백한다.
```
# 리비전 확인 option: --version
kubectl rollout history deployment echo

# 직전 버전 롤백
kubectl rollout undo deployment echo
```

7. 디플로이먼트 삭제
```
kubectl delete --f simple-deployment.yaml
```
