FROM alpine:3.7

# Install git and bash
RUN apk --no-cache add \
    bash \
    git;

# Install blackbox (https://github.com/StackExchange/blackbox)
RUN git clone https://github.com/StackExchange/blackbox.git; \
    cp blackbox/bin/* usr/local/bin/;