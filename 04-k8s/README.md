# Phase 4. Kubernetes (kubeadm)

## 클러스터 구성

| 노드 | 역할 | vCPU / Memory | IP |
| --- | --- | --- | --- |
| | control-plane | | |
| | worker | | |
| | worker | | |
| server2 | 외부 DB (MariaDB) | | |

- Kubernetes 버전:
- CNI:
- Ingress Controller:

## 매니페스트

| 파일 | 리소스 |
| --- | --- |
| `deployment.yaml` | Deployment (replicas 2, requests/limits) |
| `service.yaml` | Service |
| `ingress.yaml` | Ingress |
| `configmap.yaml` | DB URL · 사용자 |
| `secret.yaml` | DB 비밀번호 *(실제 값은 커밋하지 않음)* |
| `hpa.yaml` | HorizontalPodAutoscaler |

## 🧪 실험 기록

### 1. 배포
```bash
kubectl apply -f .
kubectl get all
```

### 2. 설정 분리 (평문 → Secret)
| Before (Phase 1) | After |
| --- | --- |
| | |

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
