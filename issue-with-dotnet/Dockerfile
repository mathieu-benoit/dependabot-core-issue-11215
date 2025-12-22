# Comment
FROM mcr.microsoft.com/dotnet/aspnet:10.0.1-noble@sha256:eaa79205c3ade4792a7f7bf310a3aac51fe0e1d91c44e40f70b7c6423d475fe0 AS setup

ARG TARGETARCH

COPY publish/$TARGETARCH /app/

RUN if [ "$TARGETARCH" = "amd64" ]; \
    then mv /app/datadog/linux-x64 /app/datadog/linux-amd64; rm -rf /app/datadog/linux-arm64; \
    elif [ "$TARGETARCH" = "arm64" ]; then rm -rf /app/datadog/linux-x64; \
    else echo "Unsupported TARGETARCH: $TARGETARCH"; exit 1; fi

# Comment
FROM mcr.microsoft.com/dotnet/aspnet:10.0.1-noble-chiseled@sha256:ba111738d21b4898f433fd8724ba1ed2e450adcb295c58f31d4137751922c83c

ARG BUILD_DATE
ARG COMMIT_SHA
ARG BUILD_URL
ARG TARGETARCH

COPY --from=setup /app /app

WORKDIR /app

ENTRYPOINT ["my", "executable"]