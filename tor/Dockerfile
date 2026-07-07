FROM debian:bookworm-slim

LABEL maintainer="Moein Darbani"

RUN apt-get update && \
    apt-get install -y --no-install-recommends tor ca-certificates && \
    rm -rf /var/lib/apt/lists/*

COPY torrc /etc/tor/torrc

RUN chown -R debian-tor:debian-tor /var/lib/tor && \
    chmod 700 /var/lib/tor

USER debian-tor

EXPOSE 9050

CMD ["tor", "-f", "/etc/tor/torrc"]
