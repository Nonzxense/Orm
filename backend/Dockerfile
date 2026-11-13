# Build stage
FROM golang:1.25-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

# main.go อยู่ใน ./cmd/ ไม่ใช่ root
RUN go build -o main ./cmd/

# Run stage
FROM alpine:3.24

RUN apk add --no-cache tzdata

RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

WORKDIR /app

COPY --from=builder /app/main /app/main

USER appuser

EXPOSE 8080

CMD ["./main"]
