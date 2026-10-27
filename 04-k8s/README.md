# Phase 4. Kubernetes (kubeadm)

## 클러스터 구성

| 노드 | 역할 | vCPU / Memory | IP |
| --- | --- | --- | --- |
| | control-plane | | |
| | worker | | |
| | worker | | |
| db01 | 외부 DB (MySQL 8.0) | | |

- Kubernetes 버전:
- CNI:
- Ingress Controller: (외부 노출: NodePort / 로드밸런서)

## 매니페스트

| 파일 | 리소스 |
| --- | --- |
| `deployment.yaml` | WAS Deployment (replicas 2, requests/limits, probes) |
| `service.yaml` | Service |
| `ingress.yaml` | Ingress — Phase 1·3 Nginx 프록시 역할 |
| `configmap.yaml` | DB URL · 사용자 |
| `secret.yaml` | DB 비밀번호 *(실제 값은 커밋하지 않음)* |
| `hpa.yaml` | HorizontalPodAutoscaler |

> PetClinic은 `MYSQL_URL` · `MYSQL_USER` · `MYSQL_PASS` 환경변수를 읽음 → ConfigMap/Secret을 `env`로 주입하면 properties 파일 수정 불필요
> db01의 MySQL 계정·방화벽은 was01 IP 기준 → Pod 트래픽이 나가는 노드 IP로 허용 대상 변경 필요

## 🧪 실험 기록

### 1. 배포
```bash
kubectl apply -f .
kubectl get deploy,pod,svc,ingress -A
```

### 2. 설정 분리
| 항목 | Phase 1 | Phase 4 |
| --- | --- | --- |
| 비밀번호 위치 | properties 평문 | Secret |
| 접근 가능 주체 | 서버 접속자 전체 | RBAC 확인 결과: |
| etcd 저장 시 암호화 | – | 확인 결과: |

```bash
kubectl get secret <name> -o yaml          # Base64 → 디코딩 가능함을 확인
kubectl auth can-i get secrets --as=<user>
```

### 3. 자가 복구
```bash
kubectl delete pod <pod-name> --force
kubectl get pods -w
```
| 항목 | Phase 3 (Compose) | Phase 4 (K8s) |
| --- | --- | --- |
| 복구 여부 / 소요 시간 | | |

### 4. 롤링 업데이트 · 롤백
```bash
kubectl set image deployment/petclinic petclinic=<image>:v2
kubectl set image deployment/petclinic petclinic=<image>:wrong-tag   # 실패 유도
kubectl rollout undo deployment/petclinic
kubectl rollout status deployment/petclinic
```
| 항목 | 결과 |
| --- | --- |
| 롤백 → 정상화 소요 시간 | |

### 5. HPA
| 시점 | CPU | Pod 수 |
| --- | --- | --- |
| 부하 전 | | |
| 부하 중 | | |
| 부하 종료 후 | | |

### 6. OOMKilled 재현
```bash
kubectl describe pod <pod-name>   # Last State: Terminated, Reason: OOMKilled
```

### 7. 트래픽 흐름 비교
| Phase | 경로 |
| --- | --- |
| 1 | Client → web01 Nginx → was01:8080 → db01 |
| 3 | Client → web 컨테이너 → was:8080 → db |
| 4 | Client → NodePort → Ingress Controller → Service → Pod → db01 |
