# Argomenti per personalizzare la versione di Debian, il repository e il tag
ARG DEBIAN_VERSION=buster
ARG GLMARK2_REPO=https://github.com/glmark2/glmark2.git
ARG GLMARK2_TAG=2021.02
LABEL maintainer="Andrea Zuccherelli <andrea@zuccherelli.net>"
# Usa l'immagine Debian specificata
FROM debian:${DEBIAN_VERSION}

# Imposta le variabili d'ambiente per il build
ENV GLMARK2_REPO=${GLMARK2_REPO} \
    GLMARK2_TAG=${GLMARK2_TAG}

# Aggiorna i pacchetti e installa le dipendenze necessarie
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    libgl1-mesa-dev \
    libegl1-mesa-dev \
    libgles2-mesa-dev \
    libpng-dev \
    libx11-dev && \
    rm -rf /var/lib/apt/lists/*

# Clona il repository glmark2 con il tag specificato
WORKDIR /usr/src
RUN git clone --branch ${GLMARK2_TAG} --depth 1 ${GLMARK2_REPO} glmark2

# Compila glmark2
WORKDIR /usr/src/glmark2
RUN ./waf configure --with-flavors=x11-gl && \
    ./waf build

# Copia il binario in una posizione accessibile
RUN cp ./build/src/glmark2 /usr/local/bin/

# Comando di default (opzionale, per test)
CMD ["/usr/local/bin/glmark2", "--version"]
