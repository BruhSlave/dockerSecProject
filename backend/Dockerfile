FROM golang:1.24-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -o dockerSecApp ./cmd/api/



FROM alpine:3.21

WORKDIR /app

RUN apk add --no-cache curl 

COPY --from=builder /app/dockerSecApp .

RUN adduser -D dockeruser
RUN chown dockeruser:dockeruser /app/dockerSecApp
USER dockeruser

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 CMD curl -f http://localhost:8080

CMD ["./dockerSecApp"]