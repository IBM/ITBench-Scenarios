FROM registry.access.redhat.com/ubi9:9.5-1744101466 AS builder

WORKDIR /workspace

RUN dnf update -y && \
    dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm && \
    dnf install -y openssl-devel zlib-devel luarocks make && \
    luarocks install luasocket

COPY src/ src/
COPY deps/ deps/
COPY Makefile Makefile

RUN make && make install

FROM registry.access.redhat.com/ubi9/ubi-minimal:9.5-1742914212

COPY --from=builder /usr/local/bin/wrk /usr/local/bin/wrk
COPY --from=builder /usr/local/lib/lua /usr/local/lib/lua
COPY --from=builder /usr/local/lib/luarocks /usr/local/lib/luarocks
COPY --from=builder /usr/local/share/lua /usr/local/share/lua

CMD ["/bin/bash"]
