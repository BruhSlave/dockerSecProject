FROM node:16-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build



FROM alpine:3.21

WORKDIR /app

COPY --from=builder /app/dist ./dist

CMD ["sh", "-c", "sleep infinity"]