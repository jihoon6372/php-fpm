# PHP-FPM Docker Environment

PHP 8.4-FPM 기반의 Docker 환경 구성 프로젝트입니다. Nginx와 PHP-FPM을 사용하여 웹 애플리케이션을 실행할 수 있습니다.

## 프로젝트 구조

```
php-fpm/
├── docker-compose.yml          # 프로덕션용 설정
├── docker-compose.local.yml    # 로컬 개발용 설정 (Nginx 포함)
├── php/
│   ├── Dockerfile             # PHP-FPM 이미지 빌드 설정
│   └── php.ini                # PHP 설정 파일
└── nginx/
    └── default.conf           # Nginx 설정 파일
```

## 기능

### PHP 확장

이 이미지에는 다음과 같은 PHP 확장이 포함되어 있습니다:

- **데이터베이스**: PDO, PDO MySQL, MySQLi
- **캐싱**: Redis
- **이미지 처리**: GD (JPEG, PNG, WebP, FreeType 지원)
- **문자열 처리**: mbstring
- **수학**: bcmath
- **파일 처리**: ZIP, EXIF
- **성능**: OPcache

### Docker 이미지

- **PHP**: `jihoon6372/php:8.4-fpm` (PHP 8.4 FPM)
- **Nginx**: `nginx:latest`
- **플랫폼**: linux/amd64

## 사용 방법

### 1. 프로덕션 환경 (PHP만 실행)

PHP-FPM 컨테이너만 실행합니다:

```bash
docker-compose up -d
```

- **포트**: 9000 (FastCGI)
- **자동 재시작**: 활성화

### 2. 로컬 개발 환경 (Nginx + PHP)

Nginx와 PHP-FPM을 함께 실행합니다:

```bash
docker-compose -f docker-compose.local.yml up -d
```

- **웹 서버 포트**: 80
- **PHP-FPM 포트**: 9000
- **웹 루트**: `/var/www/html` (공유 볼륨)

### 3. 이미지 빌드

```bash
docker-compose build
```

또는 특정 Docker Compose 파일로:

```bash
docker-compose -f docker-compose.local.yml build
```

## 설정 파일

### PHP 설정 (`php/php.ini`)

커스텀 PHP 설정은 `php/php.ini` 파일에서 수정할 수 있습니다. 이 파일은 컨테이너의 `/usr/local/etc/php/conf.d/php.ini`에 복사됩니다.

### Nginx 설정 (`nginx/default.conf`)

- **포트**: 80
- **웹 루트**: `/var/www/html`
- **기본 인덱스**: `index.php`, `index.html`, `index.htm`
- **FastCGI**: PHP 컨테이너의 9000번 포트로 프록시

## 웹 애플리케이션 배포

로컬 개발 환경에서 애플리케이션을 실행하려면:

1. 웹 파일을 `web-data` 볼륨에 복사하거나
2. `docker-compose.local.yml`을 수정하여 로컬 디렉토리를 마운트:

```yaml
volumes:
  - ./your-app:/var/www/html # 로컬 디렉토리 마운트
```

## 컨테이너 관리

### 로그 확인

```bash
docker-compose logs -f
```

### 컨테이너 재시작

```bash
docker-compose restart
```

### 컨테이너 중지

```bash
docker-compose down
```

### 볼륨과 함께 제거

```bash
docker-compose down -v
```

## PHP 정보 확인

컨테이너 실행 후, `phpinfo.php` 파일을 생성하여 PHP 정보를 확인할 수 있습니다:

```bash
# 로컬 환경
docker-compose -f docker-compose.local.yml exec php sh -c 'echo "<?php phpinfo();" > /var/www/html/index.php'
```

그 다음 브라우저에서 `http://localhost`로 접속하면 PHP 정보를 확인할 수 있습니다.

## 요구사항

- Docker
- Docker Compose v3.8 이상
