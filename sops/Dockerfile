FROM alpine:3.7

ENV GOLANG_VERSION 1.10.1
ENV CLOUD_SDK_VERSION 198.0.0
ENV PATH /google-cloud-sdk/bin:$PATH

# Install gcloud
RUN apk --no-cache add \
        curl \
        python \
        py-crcmod \
        bash \
        libc6-compat \
        openssh-client \
        git \
    && curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    tar xzf google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    rm google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    ln -s /lib /lib64 && \
    gcloud config set core/disable_usage_reporting true && \
    gcloud config set component_manager/disable_update_check true && \
    gcloud config set metrics/environment github_docker_image && \
    gcloud --version;

# Install GoLang
RUN set -eux; \
	apk add --no-cache --virtual .build-deps \
		bash \
		gcc \
		musl-dev \
		openssl \
		go; \
        wget -O go.tgz "https://golang.org/dl/go$GOLANG_VERSION.src.tar.gz"; \
            echo '589449ff6c3ccbff1d391d4e7ab5bb5d5643a5a41a04c99315e55c16bbf73ddc *go.tgz' | sha256sum -c -; \
            tar -C /usr/local -xzf go.tgz; \
            rm go.tgz; \
            \
            cd /usr/local/go/src; \
            for p in /go-alpine-patches/*.patch; do \
                [ -f "$p" ] || continue; \
                patch -p2 -i "$p"; \
            done; \
            ./make.bash; \
            \
            rm -rf /go-alpine-patches; \
            apk del .build-deps; \
            \
            export PATH="/usr/local/go/bin:$PATH"; \
            go version

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

# Install SOPS
RUN wget -O sops "https://github.com/mozilla/sops/releases/download/3.0.3/sops-3.0.3.linux"; \
    chmod +x sops; \
    mv sops /usr/local/bin;