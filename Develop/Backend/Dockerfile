# 1. 베이스 이미지
FROM python:3.10.12-slim

# 2. 작업 디렉토리
WORKDIR /app

# 3. 리눅스 필수 패키지 설치 (certifi 제거)
RUN apt-get update && apt-get install -y \
    libpq-dev \
    gcc \
    ca-certificates \
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 4. 의존성 패키지 설치 (certifi를 여기에 포함)
RUN pip install --no-cache-dir \
    uvicorn \
    fastapi \
    requests \
    certifi \
    sqlalchemy \
    python-dotenv \
    psycopg2-binary \
    "python-jose[cryptography]"

# 5. 소스 코드 복사
COPY . .

# 6. 실행 명령 (SSL 문제를 피하기 위해 python -m 방식 권장)
CMD ["python3", "-m", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]