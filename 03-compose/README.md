# Phase 3. Docker Compose

## 파일

| 파일 | 설명 |
| --- | --- |
| `docker-compose.yml` | petclinic-was + petclinic-db |
| `.env.example` | DB 접속 정보 예시 (실제 `.env`는 커밋하지 않음) |

## 배포

```bash
cp .env.example .env   # 값 수정
time docker compose up -d
docker compose ps
```

## 📏 측정

| 항목 | 결과 |
| --- | --- |
| `docker compose up` → 접속 확인 | |

## 🧪 한계 실험 — 컨테이너 장애

```bash
docker kill petclinic-was
docker compose ps        # 상태 확인
```

| `restart:` 설정 | 결과 | 캡처 |
| --- | --- | --- |
| (미설정) | | |
| | | |

## ⚠️ 한계

- [ ] 단일 호스트 — 호스트가 죽으면 서비스 전체 중단
- [ ] 장애 시 다른 노드로 재배치·헬스체크 기반 교체 불가
- [ ] 무중단 롤링 업데이트·롤백, 부하 기반 스케일링 불가

➡️ 다음 단계: 오케스트레이션 → [Phase 4](../04-k8s/)
