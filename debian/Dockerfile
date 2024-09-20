ARG DISTRO=debian
ARG SUITE=bookworm
ARG BASE_IMAGE=${DISTRO}:${SUITE}-slim

FROM ${BASE_IMAGE} AS builder
ARG TARGETARCH

ARG DEPENDENCIES="                 \
        devscripts                 \
        equivs                     \
        software-properties-common"

ARG TOOLS="                        \
        ca-certificates            \
        ccache                     \
        curl                       \
        wget"

ENV USE_CCACHE=1

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    set -ex \
    && rm -f /etc/apt/apt.conf.d/docker-clean \
    && echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' >/etc/apt/apt.conf.d/keep-cache \
    && apt-get update \
    && apt-get install -y ${DEPENDENCIES} \
    && apt-get install -y --no-install-recommends ${TOOLS} \
    && echo "no" | dpkg-reconfigure dash

WORKDIR /opt

ARG VERSION
ENV VERSION=${VERSION} \
    PATH=/usr/lib/ccache:${PATH}

RUN set -ex \
    && GALERA_VERSION=$(curl -s https://archive.mariadb.org/mariadb-${VERSION}/ | grep -oP 'galera-\K[^/]+(?=/)' | tail -n 1) \
    && echo "${GALERA_VERSION}" > /opt/galera_version.txt \
    && wget -q https://archive.mariadb.org/mariadb-${VERSION}/galera-${GALERA_VERSION}/src/galera-${GALERA_VERSION}.tar.gz \
    && wget -q http://archive.mariadb.org/mariadb-${VERSION}/source/mariadb-${VERSION}.tar.gz

FROM builder AS build-galera

WORKDIR /opt/galera

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update \
    && GALERA_VERSION=$(cat /opt/galera_version.txt) \
    && tar -xf /opt/galera-${GALERA_VERSION}.tar.gz \
    && cd galera-${GALERA_VERSION} \
    && mk-build-deps -t "apt-get -o Debug::pkgProblemResolver=yes --no-install-recommends -y" -i debian/control \
    && rm -f galera-*.deb galera-*.buildinfo galera-*.changes

RUN --mount=type=cache,target=/root/.cache/ccache \
    GALERA_VERSION=$(cat /opt/galera_version.txt) \
    && cd galera-${GALERA_VERSION} \
    && ./scripts/build.sh -p \
    && cd .. \
    && rm -rf galera-${GALERA_VERSION}

FROM builder AS build-mariadb

WORKDIR /opt/mariadb

RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update \
    && tar -xf /opt/mariadb-${VERSION}.tar.gz \
    && cd mariadb-${VERSION} \
    && AUTOBAKE_PREP_CONTROL_RULES_ONLY=true ./debian/autobake-deb.sh \
    && mk-build-deps -t "apt-get -o Debug::pkgProblemResolver=yes --no-install-recommends -y" -i debian/control \
    && rm -f mariadb-*.deb mariadb-*.buildinfo mariadb-*.changes

RUN --mount=type=cache,target=/root/.cache/ccache \
    export DEB_BUILD_OPTIONS=parallel=$(nproc) \
    && cd mariadb-${VERSION} \
    && ./debian/autobake-deb.sh \
    && cd .. \
    && rm -rf mariadb-${VERSION} mariadb_*.xz mariadb_*.dsc

FROM ${BASE_IMAGE}
ARG TARGETARCH

WORKDIR /opt/mariadb

COPY --from=build-galera /opt/galera /opt/mariadb/dist
COPY --from=build-mariadb /opt/mariadb /opt/mariadb/dist

VOLUME /dist

CMD cp -rf dist/* /dist/