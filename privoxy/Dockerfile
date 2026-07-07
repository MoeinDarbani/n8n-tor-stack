FROM debian:bookworm-slim

LABEL maintainer="Moein Darbani"

RUN apt-get update && \
    apt-get install -y --no-install-recommends privoxy ca-certificates && \
    rm -rf /var/lib/apt/lists/*

COPY config /etc/privoxy/config

EXPOSE 8118

CMD ["privoxy", "--no-daemon", "/etc/privoxy/config"]
