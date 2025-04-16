FROM registry.access.redhat.com/ubi9/go-toolset:1.23-1744294473 AS builder

WORKDIR /workspace

COPY go.sum go.sum
COPY go.mod go.mod
COPY vendor/ vendor/

COPY cmd/ cmd/
COPY dialer/ dialer/
COPY registry/ registry/
COPY services/ services/
COPY tls/ tls/
COPY tracing/ tracing/
COPY tune/ tune/

RUN CGO_ENABLED=0 GOOS=linux GO111MODULE=on go install -ldflags="-s -w" -mod=vendor ./cmd/...

FROM gcr.io/distroless/static:nonroot

WORKDIR /

COPY --from=builder /opt/app-root/src/go/bin/frontend .
COPY --from=builder /opt/app-root/src/go/bin/geo .
COPY --from=builder /opt/app-root/src/go/bin/profile .
COPY --from=builder /opt/app-root/src/go/bin/rate .
COPY --from=builder /opt/app-root/src/go/bin/recommendation .
COPY --from=builder /opt/app-root/src/go/bin/reservation .
COPY --from=builder /opt/app-root/src/go/bin/search .
COPY --from=builder /opt/app-root/src/go/bin/user .
