FROM node:16-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

ENV VUE_APP_API_URL=/api/

RUN npm run build



FROM nginx:1.28-alpine

RUN apk add --no-cache curl

COPY --from=builder /app/dist/ /usr/share/nginx/html/

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD curl -f http://localhost/momo-store/catalog || exit 1

CMD ["nginx", "-g", "daemon off;"]
