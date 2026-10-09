
ARG AWS_CLI_VERSION=2.37.10

FROM public.ecr.aws/docker/library/golang:1.27-trixie AS plugin-builder

ARG SESSION_MANAGER_PLUGIN_VERSION=1.2.835.0
ARG TARGETARCH

WORKDIR /src

RUN curl --fail --show-error --location \
        "https://github.com/aws/session-manager-plugin/archive/refs/tags/${SESSION_MANAGER_PLUGIN_VERSION}.tar.gz" \
        --output /tmp/session-manager-plugin.tar.gz \
    && tar -xzf /tmp/session-manager-plugin.tar.gz --strip-components=1 \
    && rm /tmp/session-manager-plugin.tar.gz

# Upstream uses a GOPATH workspace with vendored dependencies, not Go modules.
ENV GO111MODULE=off \
    GOPATH=/src/build/private:/src/vendor

RUN make copy-src pre-build \
    && CGO_ENABLED=0 GOOS=linux GOARCH="${TARGETARCH}" \
        go build -trimpath -ldflags="-s -w" \
        -o /out/session-manager-plugin ./src/sessionmanagerplugin-main

FROM public.ecr.aws/aws-cli/aws-cli:${AWS_CLI_VERSION}

COPY --from=plugin-builder /out/session-manager-plugin /usr/local/sessionmanagerplugin/bin/session-manager-plugin
COPY --from=plugin-builder /src/seelog_unix.xml /usr/local/sessionmanagerplugin/seelog.xml
COPY --from=plugin-builder /src/LICENSE /src/NOTICE /src/THIRD-PARTY /usr/local/sessionmanagerplugin/

RUN ln -s /usr/local/sessionmanagerplugin/bin/session-manager-plugin /usr/local/bin/session-manager-plugin \
    && mkdir -p /root/.aws /usr/local/sessionmanagerplugin/logs

ENTRYPOINT ["aws"]