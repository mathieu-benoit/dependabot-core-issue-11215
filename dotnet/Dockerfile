# Comment
FROM mcr.microsoft.com/dotnet/aspnet:10.0.2-noble@sha256:1aacc8154bc3071349907dae26849df301188be1a2e1f4560b903fb6275e481a AS setup

ARG TARGETARCH

COPY publish/$TARGETARCH /app/

RUN if [ "$TARGETARCH" = "amd64" ]; \
    then mv /app/datadog/linux-x64 /app/datadog/linux-amd64; rm -rf /app/datadog/linux-arm64; \
    elif [ "$TARGETARCH" = "arm64" ]; then rm -rf /app/datadog/linux-x64; \
    else echo "Unsupported TARGETARCH: $TARGETARCH"; exit 1; fi

# Comment
FROM mcr.microsoft.com/dotnet/aspnet:10.0.2-noble-chiseled@sha256:cc6a8adc9402e9c2c84423ee1a4c58a3098511ed5399804df0659eeafb0ae0cb

ARG BUILD_DATE
ARG COMMIT_SHA
ARG BUILD_URL
ARG TARGETARCH

COPY --from=setup /app /app

WORKDIR /app

ENTRYPOINT ["my", "executable"]