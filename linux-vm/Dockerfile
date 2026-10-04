FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /workspace

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    wget \
    unzip \
    zip \
    build-essential \
    python3 \
    python3-pip \
    nodejs \
    npm \
    xdg-utils \
    xfce4 \
    tightvncserver \
    novnc \
    websockify \
    supervisor \
    && rm -rf /var/lib/apt/lists/*

# Install OpenCode CLI
RUN mkdir -p /tmp/opencode && \
    cd /tmp/opencode && \
    curl -L "https://github.com/egoist/opencode/releases/latest/download/opencode-linux-amd64.tar.gz" -o opencode.tar.gz && \
    tar -xzf opencode.tar.gz && \
    chmod +x opencode && \
    mv opencode /usr/local/bin/opencode

RUN mkdir -p /opt/vnc && \
    printf '%s\n' '#!/bin/bash' 'export DISPLAY=:1' 'Xvfb :1 -screen 0 1024x768x24 &' 'sleep 2' 'xsetroot -display :1 -solid darkgrey' 'exec /usr/bin/startxfce4' > /opt/vnc/start-vnc.sh && \
    chmod +x /opt/vnc/start-vnc.sh

RUN useradd -m -s /bin/bash app && echo 'app:app' | chpasswd && \
    mkdir -p /workspace /home/app/.config && \
    chown -R app:app /workspace /home/app

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 3000 6080

CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
