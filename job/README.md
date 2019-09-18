### Job 실습
해당 pod에 container는 bash 설치 후 ping! 10초 후 pong!이라는 응답을 받는 애플리케이션이다.
ping!pong! 응답을 받아본다.

1. job을 실행한다.
```
kubectl apply -f simple-job.yaml
```

2. 로그 확인 (ping!pong!)
```
kubectl logs -l app=pingpong
```

3. 파드 상태 확인 (Completed: 종료됨)
```
kubectl get pod -l app=pingpong
```

---
### CronJob 실습
위 실습과 동일하며 1초마다 실행한다
1. cronjob 배포
```
kubectl apply -f simple-cronjob.yaml
```
2. job 확인
```
kubectl get job -l app=pingpong
```
3. 로그 확인
```
kubectl logs -l app=pingpong
```
