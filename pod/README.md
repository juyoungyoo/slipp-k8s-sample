### Pod 실습
pod 배포, 상태확인하고 삭제해본다.

1. pod 생성 및 배포하기
```
kubectl apply -f simple-pod.yaml
```

2. pod 안 echo 컨테이너 log확인방법
```
kubectl logs -f simple-echo -c echo
```

3. pod 상태 확인
```
kubectl get pod
```

4. pod 안 컨테이너 접근
```
kubectl exec -it simple-echo sh -c nginx  # -c(container) : 접근하려는 컨테이너 명
```

5. pod 삭제
```
# pod 삭제
kubectl delete pod simple-echo

# 매니페스트 파일로 삭제
kubectl delete simple-pod.yaml
```
