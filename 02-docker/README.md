# Phase 2. Docker 이미지화

> DB는 Phase 1의 `db01` MySQL을 그대로 사용

## 파일

| 파일 | 설명 |
| --- | --- |
| `Dockerfile.single` | WAS 단일 스테이지 — JDK 이미지에서 빌드 + 실행 |
| `Dockerfile.multi` | WAS 멀티스테이지 — JDK로 빌드, JRE 이미지에 jar만 복사 |
| `nginx/default.conf` | WEB 컨테이너용 프록시 설정 (`proxy_pass` 대상 = WAS 컨테이너) |

## 빌드

```bash
docker build -f Dockerfile.single -t petclinic:single .
docker build -f Dockerfile.multi  -t petclinic:multi  .
docker images | grep petclinic
```

## 실행 확인 (WEB → WAS → db01)

```bash
docker network create petclinic-net
docker run -d --name was --network petclinic-net petclinic:multi
docker run -d --name web --network petclinic-net -p 80:80 \
  -v $PWD/nginx/default.conf:/etc/nginx/conf.d/default.conf:ro nginx
```

> db01 방화벽·MySQL 계정은 was01 IP 기준 → Docker 호스트 IP가 다르면 허용 대상 수정 필요

## 📏 측정

| 이미지 | 베이스 이미지 | 크기 | 빌드 시간 |
| --- | --- | --- | --- |
| petclinic:single | | | |
| petclinic:multi | | | |
| **감소율** | | | |

📸 캡처: `docs/images/phase2-image-size.png`

## ⚠️ 한계

- [ ] WEB·WAS 컨테이너, 네트워크, 환경변수를 매번 `docker run`으로 수동 지정
- [ ] 실행 순서(WAS → WEB)를 사람이 기억해야 함

➡️ 다음 단계: 3-Tier를 파일 하나로 선언 → [Phase 3](../03-compose/)
