# Use a current, stable Debian base image
# docker build $(cat .env | sed 's/^/--build-arg /') ./ruby/2.3.6/. -t robsdudeson/ruby:2.3.6-bookworm-slim
FROM debian:bookworm-slim

# Set environment variables for Ruby installation
ENV RUBY_DOWNLOAD_SHA256=8322513279f9edfa612d445bc111a87894fac1128eaa539301cebfc0dd51571e
ENV RUBY_SRC=/usr/src/ruby
ENV RUBY_TAR_BALL=/tmp/ruby.tar.gz
ENV RUBY_VERSION=2.3.6
ENV RUBY_URL=https://cache.ruby-lang.org/pub/ruby/2.3/ruby-$RUBY_VERSION.tar.gz

# Install build dependencies and download Ruby
RUN set -ex \
    \
    # Install necessary build tools and libraries for Ruby
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libyaml-dev \
        libreadline-dev \
        zlib1g-dev \
        ca-certificates \
        wget \
    \
    # Download Ruby source
    && wget -O $RUBY_TAR_BALL $RUBY_URL \
    \
    # Verify checksum
    && echo "$RUBY_DOWNLOAD_SHA256 *${RUBY_TAR_BALL}" | sha256sum -c - \
    \
    # Extract and compile Ruby
    && mkdir -p $RUBY_SRC \
    && tar -xzf $RUBY_TAR_BALL -C $RUBY_SRC --strip-components=1 \
    && cd $RUBY_SRC \
    \
    && ./configure --prefix=/usr/local --disable-install-doc \
    && make -j"$(nproc)" \
    && make install \
    \
    # Clean up environment variables
    && unset LDFLAGS CPPFLAGS \
    \
    # Clean up build dependencies and source code
    && apt-get purge -y --auto-remove build-essential wget \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf $RUBY_SRC \
    && rm $RUBY_TAR_BALL

# Set default command (optional, adjust as needed for your application)
CMD ["ruby", "-v"]