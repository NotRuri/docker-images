FROM --platform=$TARGETOS/$TARGETARCH debian:bookworm-slim

LABEL version="1.0" \
      description="Pelican container for open.mp" \
      author="Ruri" \
      maintainer="github.com/NotRuri"

RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y \
    libatomic1:i386 \
    libc6:i386 \
    libicu-dev:i386 \
    libmariadb3:i386 \
    libncurses6:i386 \
    libstdc++6:i386 \
    libtbb-dev:i386 \
    libtbb12:i386 \
    libtinfo6:i386 \
    tar wget curl iproute2 openssl fontconfig dirmngr \
    ca-certificates dnsutils tzdata zip tini netcat-openbsd

RUN if [ "$(uname -m)" = "x86_64" ]; then \
        wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_i386.deb && \
        dpkg -i libssl1.1_1.1.0g-2ubuntu4_i386.deb && \
        rm libssl1.1_1.1.0g-2ubuntu4_i386.deb; \
    fi

RUN mkdir -p /run/systemd && echo 'docker' > /run/systemd/container

RUN rm -rf /var/lib/apt/lists/*

# ---- start pelican stuff ---- #

RUN         useradd -d /home/container -m container

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL  SIGINT

COPY        --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT  ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]

# ----- end pelican stuff ----- #
