# Base image with build tools
FROM mcr.microsoft.com/devcontainers/base:ubuntu@sha256:94995b41dcfcfd9f12de62793546ace86417b846913ad0a23daa1abc51630890

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    clang \
    gdb \
    curl \
    wget \
    git \
    gnupg \
    unzip \
    apt-transport-https \
    ca-certificates \
    libboost-all-dev \
    software-properties-common \
    libpq-dev \
    postgresql \
    postgresql-client \
    postgresql-server-dev-all \
    postgresql-contrib \
    redis-server \
    openssl \
    libssl-dev \
    python3 \
    python3-pip \
    default-jdk \
    && rm -rf /var/lib/apt/lists/*

# Install Bazel
RUN curl -fsSL https://github.com/bazelbuild/bazel/releases/download/8.2.0rc3/bazel-8.2.0rc3-linux-arm64 -o /usr/local/bin/bazel \
    && chmod +x /usr/local/bin/bazel

# Install Buildifier
RUN curl -fsSL https://github.com/bazelbuild/buildtools/releases/download/v8.0.3/buildifier-linux-arm64 -o /usr/local/bin/buildifier && \
    chmod +x /usr/local/bin/buildifier

# Install Buildozer
RUN curl -fsSL https://github.com/bazelbuild/buildtools/releases/download/v8.0.3/buildozer-linux-arm64 -o /usr/local/bin/buildozer && \
    chmod +x /usr/local/bin/buildozer

# Configure Redis and PostgreSQL to run as services in the container
RUN systemctl enable redis-server && \
    systemctl enable postgresql


# Setup Oat++ manually
RUN cd opt && git clone --depth=1 https://github.com/oatpp/oatpp && \
    cd oatpp && mkdir build && cd build && \
    cmake .. && make -j$(nproc) && make install

# Setup Oat++ Swagger module
RUN cd opt && git clone --depth=1 https://github.com/oatpp/oatpp-swagger && \
    cd oatpp-swagger && mkdir build && cd build && \
    cmake .. && make -j$(nproc) && make install

# Setup Oat++ PostgreSQL module
RUN cd opt && git clone --depth=1 https://github.com/oatpp/oatpp-postgresql && \
    cd oatpp-postgresql && mkdir build && cd build && \
    cmake -DPostgreSQL_TYPE_INCLUDE_DIR=/usr/include/postgresql -DOATPP_BUILD_TESTS=OFF .. && \
    make -j$(nproc) && make install

# Setup user and working dir
RUN id -u vscode &>/dev/null || (useradd -m vscode && echo "vscode:vscode" | chpasswd && adduser vscode sudo)
USER vscode
WORKDIR /workspace