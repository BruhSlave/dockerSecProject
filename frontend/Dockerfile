FROM node:16-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

RUN npm run build



FROM nginx:1.28-alpine

RUN apk add --no-cache curl 

COPY --from=builder /app/dist /var/www/html/

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 CMD curl -f http://localhost:80

CMD ["nginx", "-g", "daemon off;"]