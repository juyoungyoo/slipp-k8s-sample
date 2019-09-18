### Service 실습
3개의 pod(summer, summer, spring) 존재한다. summer인 pod에만 접근 가능한 서비스를 만들어본다.

1. 먼저 pod 3개 배포하고 정상적으로 배포되었는지 확인한다
summer(1), spring(2)
```
# 배포
kubectl apply -f simple-replicaset-with-label.yaml

# pod 확인
kubectl apply get pod -l app=echo -l release=spring

kubectl apply get pod -l app=echo -l release=summer
```

2. release=summer인 파드만 접근할 수 있는 서비스 생성하고 확인한다
```
# 서비스 생성
kubectl apply -f simple-service.yaml

# 서비스 확인
kubectl get svc echo
```

3. 트래픽 전달 확인
클러스터 내부에서 요청을 보내 summer pod에 도달하였는지 확인한다    
쿠버네티스 클러스터 안에서만 접근할 수 있으므로 쿠버네티스 클러스터에서 디버깅용 임시 컨테이너를 배포하고 확인한다.    
```
# 디버깅용 임시 컨테이너 배포
kubectl run -i --rm --tty debug --image=gihyodocker/fundamental:0.1.0 --restart=Never -- bash -il
```
```
# HTTP 요청
curl http://echo/
```

4. 레이블이 summer인 파드에 해당 파드에 접근하였는지 로그 확인
```
# summer pod명 복사하여 확인
kubectl logs -f [echo summer pod name] -c echo
```

위 방법은 cluster IP로 컨테이너 내부에서만 서비스 접이 가능하였다.
nodeport 서비스로 외부에서 접근하여 위 예제를 실행해본다.
5. nodeport 생성
```
kubectl apply -f simple-NodePort.yaml
```

6. 서비스 확인
```
# service 확인
kubectl get svc echo

# test
curl http://localhost:30725
```
