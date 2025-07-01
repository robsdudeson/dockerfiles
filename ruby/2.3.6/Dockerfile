# Use a current, stable Debian base image
# docker build $(cat .env | sed 's/^/--build-arg /') ./ruby/2.3.6/. -t robsdudeson/ruby:2.3.6-bookworm-slim
FROM debian:bookworm-slim

# Install build dependencies for OpenSSL and Ruby
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    perl \
    zlib1g-dev \
    ca-certificates \
    wget \
    libyaml-dev \
    libssl-dev \
    libreadline-dev \
    && rm -rf /var/lib/apt/lists/*

# Download and extract OpenSSL 1.1.1w
WORKDIR /usr/local/src
ARG OPENSSL_VERSION=1.1.1w
ARG OPENSSL_DOWNLOAD_URL=https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz
ARG OPENSSL_SHA256=cf3098950cb4d853ad95c0841f1f9c6d3dc102dccfcacd521d93925208b76ac8

RUN wget -q ${OPENSSL_DOWNLOAD_URL} -O openssl-${OPENSSL_VERSION}.tar.gz \
    && echo "${OPENSSL_SHA256}  openssl-${OPENSSL_VERSION}.tar.gz" | sha256sum -c - \
    && tar -xzf openssl-${OPENSSL_VERSION}.tar.gz \
    && rm openssl-${OPENSSL_VERSION}.tar.gz

# Compile and install OpenSSL
WORKDIR /usr/local/src/openssl-1.1.1w
RUN ./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl shared zlib \
    && make \
    && make install

# Configure dynamic linker to find OpenSSL libraries
RUN echo "/usr/local/openssl/lib" > /etc/ld.so.conf.d/openssl-1.1.1.conf \
    && ldconfig

# Download and extract Ruby 2.3.6
WORKDIR /usr/local/src
ARG RUBY_VERSION=2.3.6
ARG RUBY_DOWNLOAD_URL=https://cache.ruby-lang.org/pub/ruby/2.3/ruby-${RUBY_VERSION}.tar.gz
ARG RUBY_SHA256=8322513279f9edfa612d445bc111a87894fac1128eaa539301cebfc0dd51571e

RUN wget -q ${RUBY_DOWNLOAD_URL} -O ruby-${RUBY_VERSION}.tar.gz \
    && echo "${RUBY_SHA256}  ruby-${RUBY_VERSION}.tar.gz" | sha256sum -c - \
    && tar -xzf ruby-${RUBY_VERSION}.tar.gz \
    && rm ruby-${RUBY_VERSION}.tar.gz

# Compile and install Ruby
WORKDIR /usr/local/src/ruby-2.3.6
RUN ./configure \
    --prefix=/usr/local \
    --with-openssl-dir=/usr/local/openssl \
    --disable-install-doc \
    --enable-shared \
    && make \
    && make install

# Clean up build dependencies and temporary files
RUN apt-get purge -y --auto-remove build-essential perl zlib1g-dev wget libyaml-dev libssl-dev libreadline-dev \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/local/src/*

# Set default command (optional, adjust as needed for your application)
CMD ["ruby", "-v"]