FROM debian:bookworm-slim as builder
ARG TARGETARCH

ARG BUILD_DEPENDENCIES="           \
        ca-certificates            \
        curl                       \
        devscripts                 \
        software-properties-common \
        wget"

RUN set -ex \
    && apt-get update \
    && apt-get install -y ${BUILD_DEPENDENCIES} \
    && echo "no" | dpkg-reconfigure dash \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

FROM builder as build-galera
ARG TARGETARCH
ARG VERSION=10.11.6

WORKDIR /opt/galera

RUN set -ex \
    && GALERA_VERSION=$(curl -s https://archive.mariadb.org/mariadb-${VERSION}/ | grep -oP 'galera-\K[^/]+(?=/)' | tail -n 1) \
    && echo "${GALERA_VERSION}" > /opt/galera_version.txt

RUN set -ex \
    && GALERA_VERSION=$(cat /opt/galera_version.txt) \
    && wget -q https://archive.mariadb.org/mariadb-${VERSION}/galera-${GALERA_VERSION}/src/galera-${GALERA_VERSION}.tar.gz \
    && tar -xf galera-${GALERA_VERSION}.tar.gz \
    && rm -f galera-${GALERA_VERSION}.tar.gz

RUN set -ex \
    && apt-get update \
    && GALERA_VERSION=$(cat /opt/galera_version.txt) \
    && cd galera-${GALERA_VERSION} \
    && mk-build-deps -t "apt-get -o Debug::pkgProblemResolver=yes --no-install-recommends -y" -i debian/control

RUN set -ex \
    && GALERA_VERSION=$(cat /opt/galera_version.txt) \
    && cd galera-${GALERA_VERSION} \
    # https://github.com/codership/galera/issues/652
    && sed -i "s@dh_auto_test @# dh_auto_test @g" debian/rules \
    && ./scripts/build.sh -p \
    && cd .. \
    && rm -rf galera-${GALERA_VERSION}

FROM builder as build-mariadb
ARG TARGETARCH
ARG VERSION=10.11.6

WORKDIR /opt/mariadb

RUN set -ex \
    && wget -q http://archive.mariadb.org/mariadb-${VERSION}/source/mariadb-${VERSION}.tar.gz \
    && tar -xf mariadb-${VERSION}.tar.gz \
    && rm -f mariadb-${VERSION}.tar.gz

RUN set -ex \
    && apt-get update \
    && cd mariadb-${VERSION} \
    && AUTOBAKE_PREP_CONTROL_RULES_ONLY=true ./debian/autobake-deb.sh \
    && mk-build-deps -t "apt-get -o Debug::pkgProblemResolver=yes --no-install-recommends -y" -i debian/control

RUN set -ex \
    && export DEB_BUILD_OPTIONS=parallel=$(nproc) \
    && cd mariadb-${VERSION} \
    && ./debian/autobake-deb.sh \
    && cd .. \
    && rm -rf mariadb-${VERSION}

FROM debian:bookworm-slim
ARG TARGETARCH

WORKDIR /opt/mariadb

COPY --from=build-galera /opt/galera /opt/mariadb/dist
COPY --from=build-mariadb /opt/mariadb /opt/mariadb/dist

VOLUME /dist

CMD cp -rf dist/* /dist/