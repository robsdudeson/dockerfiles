# Use a current, stable Debian base image
FROM debian:bookworm-slim

# Install build dependencies for OpenSSL and Ruby
RUN apt-get update -qq \
    && apt-get install -y --no-install-recommends \
        build-essential \
        ca-certificates \
        curl \
        git \
        libreadline-dev \
        libyaml-dev \
        perl \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Install rbenv and ruby-build
ENV HOME=/root
ENV RBENV_ROOT=${HOME}/.rbenv
ENV PATH=${RBENV_ROOT}/bin:${RBENV_ROOT}/shims:$PATH

RUN git clone https://github.com/rbenv/rbenv.git ${RBENV_ROOT} \
    && git clone https://github.com/rbenv/ruby-build.git ${RBENV_ROOT}/plugins/ruby-build

# Install Ruby 2.3.6 using rbenv
ARG RUBY_VERSION=2.3.6
RUN rbenv install ${RUBY_VERSION} \
    && rbenv global ${RUBY_VERSION} \
    && gem update --system 3.2.3 --no-document \
    && gem install bundler -v 2.3.27 --no-document \
    && rbenv rehash

# Clean up build dependencies and temporary files
RUN apt-get purge -y --auto-remove \
    build-essential \
    curl \
    libreadline-dev \
    perl \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/local/src/* \
    && rm -rf ${RBENV_ROOT}/versions/${RUBY_VERSION}/share/doc \
    && rm -rf ${RBENV_ROOT}/versions/${RUBY_VERSION}/lib/ruby/gems/*/cache/*.gem \
    && rm -rf ${RBENV_ROOT}/versions/${RUBY_VERSION}/lib/ruby/gems/*/specifications/*.gemspec

# Set default command (optional, adjust as needed for your application)
CMD ["ruby", "-v"]
