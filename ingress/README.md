### Ingress 실습
Ingress controller를 이용하여 URI, Host 기반의 라우팅을 해본다

1. Ingress controller 배포
ingress-nginx를 사용하며 'ingress-nginx'라는 namespace로 생성
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml
```

2. Ingress service 생성
외부에서 접근 가능하도록 Loadbalancer 타입의 Ingress service 생성하고 확인한다
```
# Ingress service 생성
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/cloud-generic.yaml
```
```
# 로드밸런서 생성 여부 실시간 확인 (External IP 확인)
kubectl get service -n ingress-nginx --watch
```

3. Application 및 서비스를 배포
다른 정보를 가진 deployment, service를 생성한다.
예제용 deployment는 자신의 host 정보를 보여주는 nginx container이다. (각 서비스는 Cluster IP로 외부접근 불가)
 -  deployment : apps/a, apps/b
 -  service : service/a-svc, service/b-svc
```
kubectl apply -f https://gist.githubusercontent.com/NaverCloudPlatformDeveloper/c47620d8d25b2a0e08648f225043adf6/raw/675addde0a56ba727824f30f92a73b40550fb73c/nks-tutorial-hello-service.yaml
```

4. URI 기반 라우팅
Path별로 라우팅하여 /a, /b 요청 시 각각 다른 서비스로 연결되도록 설정한다.   
http://localhost/a → a-svc 서비스로 접근    
http://localhost/b→ b-svc 서비스로 접근   

4-1) Ingress 생성 ( sample_ingress_uri.yaml 배포 )
```
kubectl apply -f sample_ingress_uri.yaml
```

4-2. Ingress 생성 확인 후 URI 라우팅 테스트
```
# Ingress 생성 확인
kubectl get ingress -n ingress-nginx --watch
```
```
# 서비스 접속확인
curl http://localhost/a
curl http://localhost/b
```

5. 호스트기반 라우팅
Host 기반 라우팅을 테스트 한다.    
동일한 External-IP에 각각 다른 도메인으로 요청 시 다른 서비스로 연결되도록 설정한다    
> a.svc.com으로 요청 시  → a-svc 서비스로 접근   
b.svc.com으로 요청 시→ b-svc 서비스로 접근   

5-0. 이전 URI Ingress 설정 삭제   
Ingress 설정 제거
```  
kubectl delete ingress ab-ingress
```

5-1. Ingress 생성 ( sample_ingress_host.yaml 배포 )
```
kubectl apply -f sample_ingress_host.yaml
```

5-2. Ingress 생성 확인 후 URI 라우팅 테스트    
```
# Ingress 생성 확인
kubectl get ingress
```

```
# 서비스 접속확인
curl -H host:a.svc.com http://localhost.com
curl -H host:b.svc.com http://localhost.com
```
