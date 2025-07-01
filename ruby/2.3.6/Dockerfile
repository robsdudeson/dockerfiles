FROM debian:bookworm-slim

ARG RUBY_VERSION=2.3.6

# Install packages needed for ruby-build
RUN apt-get update -qq \
    && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
        build-essential \
        ca-certificates \
        curl \
        git \
        libz-dev

# Check out ruby-build and install
RUN git clone https://github.com/sstephenson/ruby-build.git /usr/local/ruby-build
RUN /usr/local/ruby-build/install.sh

# Install the version of Ruby we want and add to the path
RUN ruby-build ${RUBY_VERSION} /usr/local/ruby-${RUBY_VERSION}
ENV PATH="/usr/local/ruby-${RUBY_VERSION}/bin:${PATH}"

RUN gem update --system 3.2.3 --no-document \
    && gem install bundler -v 2.3.27 --no-document

CMD ["ruby", "-v"]
