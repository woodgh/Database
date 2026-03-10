# Database

VPS에서 실행하는 공용 MySQL 서버. 같은 VPS의 여러 프로젝트가 Docker 네트워크를 통해 접속한다.

## 설치

```bash
# 1. 공유 네트워크 생성 (최초 1회)
docker network create shared

# 2. .env 수정
vi .env

# 3. 실행
docker compose up -d
```

## 환경변수 (.env)

```bash
MYSQL_ROOT_PASSWORD=changeme   # 반드시 변경
```

## 다른 프로젝트에서 접속

### Docker 네트워크 (같은 VPS 컨테이너)

`docker-compose.yml`에 `shared` 네트워크를 추가하고, 호스트를 컨테이너명 `mysql`로 지정한다.

```yaml
networks:
  shared:
    external: true
```

```bash
DB_HOST=mysql
DB_PORT=3306
```

### 호스트에서 직접 접속 (같은 VPS)

```bash
DB_HOST=127.0.0.1
DB_PORT=3306
```

## 보안

- `127.0.0.1:3306`으로 바인딩되어 **외부 접속 불가**
- `shared` 네트워크는 Docker 내부 전용으로 외부에 노출되지 않음
