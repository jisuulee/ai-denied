# 1. 빌드 스테이지
FROM node:20-alpine AS build-stage
WORKDIR /app
# 의존성 먼저 설치 (캐시 활용)
COPY package*.json ./
RUN npm install
# 소스 복사 전, 빌드 결과물이 섞이지 않게 함
COPY . .
# [중요] 빌드 시점에 시간을 찍어서 파일이 바뀌었음을 강제로 알림
RUN echo "Build Date: $(date)" > /app/dist/build_time.txt
RUN npm run build

# 2. 실행 스테이지
FROM nginx:stable-alpine
# 이전 레이어 캐시 무시를 위해 설정 파일 복사
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
# 빌드 스테이지의 결과물만 깨끗하게 가져옴
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]