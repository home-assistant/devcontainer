FROM mcr.microsoft.com/devcontainers/base:1-debian

ENV \
    DEBIAN_FRONTEND=noninteractive \
    DEVCONTAINER=1

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install tools
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        dbus \
        network-manager \
        libpulse0 \
        xz-utils

COPY ./common/rootfs /
COPY ./common/rootfs_supervisor /
COPY ./common/install /tmp/common/install

# Install common
RUN \
    bash devcontainer_init \
    && common_install_packages \
        docker \
        shellcheck \
        cosign \
        os-agent \
    && usermod -aG docker vscode

COPY ./addons/rootfs /
