FROM golang:1.24.3-alpine AS builder

WORKDIR /todo

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -o todo ./cmd/main.go

FROM alpine:latest

RUN adduser -D appuser

WORKDIR /app

COPY --from=builder /todo/todo .

USER appuser

EXPOSE 9000

ENTRYPOINT ["./todo"]
