FROM mcr.microsoft.com/dotnet/aspnet:8.0.22-noble@sha256:27f5cfd3f9e294a490ca6a7681dc2bf3a91bf5dc62d2b2eaa77a734d0f85387a AS setup

ARG TARGETARCH

COPY publish/$TARGETARCH /app/

RUN if [ "$TARGETARCH" = "amd64" ]; \
    then mv /app/datadog/linux-x64 /app/datadog/linux-amd64; rm -rf /app/datadog/linux-arm64; \
    elif [ "$TARGETARCH" = "arm64" ]; then rm -rf /app/datadog/linux-x64; \
    else echo "Unsupported TARGETARCH: $TARGETARCH"; exit 1; fi

FROM mcr.microsoft.com/dotnet/aspnet:8.0.22-noble-chiseled@sha256:a68ac644781dac794255e06cdd331be696de01137b1ccd2e753e06851705dd3b

ARG BUILD_DATE
ARG COMMIT_SHA
ARG BUILD_URL
ARG TARGETARCH

COPY --from=setup /app /app

WORKDIR /app

ENTRYPOINT ["my", "executable"]