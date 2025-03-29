# Argomenti per personalizzare la versione di Debian, il repository e il tag
ARG DEBIAN_VERSION=buster
# Usa l'immagine Debian specificata
FROM debian:${DEBIAN_VERSION}
LABEL maintainer="Andrea Zuccherelli <andrea@zuccherelli.net>"
ARG GLMARK2_REPO=https://github.com/glmark2/glmark2.git
ARG GLMARK2_TAG=2021.02

# Imposta le variabili d'ambiente per il build
ENV GLMARK2_REPO=${GLMARK2_REPO} \
    GLMARK2_TAG=${GLMARK2_TAG}

# Aggiorna i pacchetti e installa le dipendenze necessarie, incluso meson e ninja
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    libgl1-mesa-dev \
    libegl1-mesa-dev \
    libgles2-mesa-dev \
    libpng-dev \
    libx11-dev \
    meson \
    ninja-build \
    pkg-config && \
    rm -rf /var/lib/apt/lists/*

# Clona il repository glmark2 con il tag specificato
WORKDIR /usr/local/src
RUN git clone --branch ${GLMARK2_TAG} --depth 1 ${GLMARK2_REPO} glmark2

# Compila glmark2 con meson
WORKDIR /usr/local/src/glmark2
RUN meson setup build -Dflavors=x11-gl && \
    ninja -C build

# Copia il binario
RUN cp ./build/src/glmark2 /usr/local/bin/

# Comando di default (opzionale)
CMD ["/usr/local/bin/glmark2", "--version"]
