# MinIO's community edition dropped prebuilt Docker images (see
# https://github.com/minio/minio — "source code only" now). This builds
# both the server (minio) and client (mc) from source ourselves, same
# license (AGPLv3), just built by us instead of MinIO.
FROM golang:1.24-bookworm AS builder
RUN go install github.com/minio/minio@latest && \
    go install github.com/minio/mc@latest

FROM debian:bookworm-slim
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates curl \
    && rm -rf /var/lib/apt/lists/*
COPY --from=builder /go/bin/minio /usr/bin/minio
COPY --from=builder /go/bin/mc /usr/bin/mc
RUN mkdir -p /data
VOLUME ["/data"]
EXPOSE 9000 9001
ENTRYPOINT ["minio"]
CMD ["server", "/data", "--console-address", ":9001"]
