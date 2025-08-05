# Usa imagem oficial de C++ com Debian
FROM mcr.microsoft.com/devcontainers/cpp:1-debian-12

# Instala dependências para o Bazelisk e o Bazel
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    unzip \
    zip \
    bash \
    git \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Instala o Bazelisk (wrapper para gerenciar versões do Bazel)
RUN curl -Lo /usr/local/bin/bazelisk https://github.com/bazelbuild/bazelisk/releases/latest/download/bazelisk-linux-amd64 \
    && chmod +x /usr/local/bin/bazelisk \
    && ln -s /usr/local/bin/bazelisk /usr/local/bin/bazel