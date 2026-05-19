# 1. 베이스 이미지 설정
FROM python:3.10.12-slim

# 2. 작업 디렉토리 생성
WORKDIR /app

# 3. 필수 리눅스 패키지 및 CA 인증서 설치
RUN apt-get update && apt-get install -y \
    libpq-dev \
    gcc \
    ca-certificates \
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 4. 의존성 패키지 설치 (certifi 포함)
RUN pip install --no-cache-dir \
    uvicorn \
    fastapi \
    requests \
    certifi \
    sqlalchemy \
    python-dotenv \
    psycopg2-binary \
    "python-jose[cryptography]"

# 5. 소스 코드 복사 및 인증서 갱신
COPY . .
RUN update-ca-certificates

# 6. 실행 명령 (모듈 호출 및 절대 경로 사용)
CMD ["python3", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]