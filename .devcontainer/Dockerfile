FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    ca-certificates \
    curl \
    git \
    make \
    gcc \
    g++ \
    python3 \
    python3-pip \
    python3-venv \
    xz-utils \
    tar \
    liblz4-dev \
    zlib1g-dev \
    locales \
    && locale-gen en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/*

# Pin OSS CAD Suite so everyone gets the same tools.
ARG OSS_CAD_TAG=2026-10-07

RUN set -eux; \
    URL="$(curl -fsSL \
        "https://api.github.com/repos/YosysHQ/oss-cad-suite-build/releases/tags/${OSS_CAD_TAG}" \
        | python3 -c \
        'import sys,json; r=json.load(sys.stdin); print(next(a["browser_download_url"] for a in r["assets"] if "linux-x64" in a["name"] and a["name"].endswith(".tgz")))')"; \
    curl -fsSL "${URL}" -o /tmp/oss-cad-suite.tgz; \
    tar -xzf /tmp/oss-cad-suite.tgz -C /opt; \
    rm /tmp/oss-cad-suite.tgz

ENV PATH="/opt/oss-cad-suite/bin:${PATH}"
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

WORKDIR /workspaces/systemverilog

CMD ["/bin/bash"]