FROM mcr.microsoft.com/devcontainers/python:1-3.12

ENV \
    DEBIAN_FRONTEND=noninteractive \
    DEVCONTAINER=1 \
    NVM_DIR="/root/.nvm" \
    UV_SYSTEM_PYTHON=true

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install tools
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        dbus \
        dbus-x11 \
        network-manager \
        libpulse0 \
        xz-utils

COPY ./common/rootfs /
COPY ./common/rootfs_supervisor /
COPY ./common/install /tmp/common/install

# Install common
RUN \
    pip3 install uv==0.2.15 \
    bash devcontainer_init \
    && common_install_packages \
        docker \
        shellcheck \
        cosign \
        os-agent \
        yarn

RUN usermod -aG docker vscode

COPY ./supervisor/rootfs /
