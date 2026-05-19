# 1. 빌드 스테이지
FROM node:20-alpine AS build-stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# [교정] 빌드를 먼저 실행해서 dist 폴더를 만든 후, 그 안에 시간 파일을 기록합니다.
RUN npm run build
RUN echo "Build Date: $(date)" > /app/dist/build_time.txt

# 2. 실행 스테이지
FROM nginx:stable-alpine
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
# 빌드 스테이지에서 생성된 깨끗한 결과물만 복사
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]