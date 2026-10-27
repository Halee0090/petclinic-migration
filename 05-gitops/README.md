# Phase 5. GitOps

> 수업에서 ArgoCD 학습 후 진행

## 파일 (예정)

| 파일 | 설명 |
| --- | --- |
| `../.github/workflows/build.yml` | 이미지 빌드 → GHCR push → 매니페스트 태그 갱신 |
| `argocd-application.yaml` | `04-k8s/` 경로를 감시하는 ArgoCD Application |

## 체크리스트

- [ ] GitHub Actions로 이미지 빌드·푸시
- [ ] ArgoCD 설치 및 Application 등록
- [ ] git push → 클러스터 반영까지 소요 시간 측정
- [ ] (선택) EKS 확장
