FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY dispatcher.go .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" dispatcher.go
FROM scratch
WORKDIR /app
COPY --from=builder /app/dispatcher .
COPY static/ static/
EXPOSE 80
CMD ["./dispatcher"]