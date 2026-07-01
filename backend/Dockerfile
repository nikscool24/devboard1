# Stage 1: build the Go binary
FROM dhi.io/golang:1-alpine AS builder

WORKDIR /app

# Download deps first for better layer caching
COPY go.mod go.sum ./
RUN go mod download || go mod download

# Build the binary
COPY . .
RUN CGO_ENABLED=0 go build -o devboard-backend .

# Stage 2: small runtime image
FROM dhi.io/alpine-base:3.24

WORKDIR /app

COPY --from=builder /app/devboard-backend .

RUN chmod +x ./devboard-backend && \
    adduser -D -u 65532 nonroot

USER nonroot

EXPOSE 8080

ENTRYPOINT ["./devboard-backend"]
