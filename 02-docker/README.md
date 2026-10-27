# Phase 2. Docker 이미지화

## 파일

| 파일 | 설명 |
| --- | --- |
| `Dockerfile.single` | 단일 스테이지 — JDK 이미지에서 빌드 + 실행 |
| `Dockerfile.multi` | 멀티스테이지 — JDK로 빌드, JRE 이미지에 jar만 복사 |

## 빌드

```bash
docker build -f Dockerfile.single -t petclinic:single .
docker build -f Dockerfile.multi  -t petclinic:multi  .
docker images | grep petclinic
```

## 📏 측정

| 이미지 | 베이스 이미지 | 크기 | 빌드 시간 |
| --- | --- | --- | --- |
| petclinic:single | | | |
| petclinic:multi | | | |
| **감소율** | | | |

📸 캡처: `docs/images/phase2-image-size.png`

## ⚠️ 한계

- [ ] WAS·DB 컨테이너, 네트워크, 환경변수를 매번 `docker run`으로 수동 지정

➡️ 다음 단계: 구성을 파일 하나로 선언 → [Phase 3](../03-compose/)
