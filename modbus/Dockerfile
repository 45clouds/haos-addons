ARG BUILD_FROM
FROM $BUILD_FROM

# Setup base
RUN \
    set -x \ 
    && apk add --no-cache \
        bash-completion \
        git \
        libuv \
        mosquitto-clients \
        nano \
        pwgen \
        vim \
        joe \
        mc \
        python3 \
        py3-pip \
        py3-paho-mqtt \
        py3-coverage \
        py3-coveralls \
    \
    && apk add --no-cache --virtual .build-dependencies \
        bsd-compat-headers \
        build-base \
        linux-headers \
        cmake \
        json-c-dev \
        libuv-dev \
        openssl-dev \
        zlib-dev \
    \
    && sed -i "s/ash/bash/" /etc/passwd \
    \
    && git clone --branch main --single-branch \
         https://github.com/45clouds/isystem.git /tmp/isystem \
    \
    && cd /tmp/isystem \
    && python3 setup.py install \
    \
    && apk del --no-cache --purge .build-dependencies \
    && rm -f -r \
        /root/.cache \
        /root/.cmake \
        /tmp/*

# Add YAML highlighting for nano
ADD https://raw.githubusercontent.com/scopatz/nanorc/master/yaml.nanorc /usr/share/nano/yaml.nanorc
RUN sed -i 's/^#[[:space:]]*\(include "\/usr\/share\/nano\/\*\.nanorc".*\)/\1/' /etc/nanorc

# Home Assistant CLI
ARG BUILD_ARCH
ARG CLI_VERSION
RUN curl -Lso /usr/bin/ha \
        "https://github.com/home-assistant/cli/releases/download/${CLI_VERSION}/ha_${BUILD_ARCH}" \
    && chmod a+x /usr/bin/ha \
    && /usr/bin/ha completion > /usr/share/bash-completion/completions/ha

# Copy data
COPY rootfs /
