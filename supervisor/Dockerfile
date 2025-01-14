FROM mcr.microsoft.com/devcontainers/python:1-3.13

ENV \
    DEBIAN_FRONTEND=noninteractive \
    DEVCONTAINER=1 \
    NVM_DIR="/root/.nvm"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install tools
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        dbus-daemon \
        network-manager \
        libpulse0 \
        xz-utils

COPY ./common/rootfs /
COPY ./common/rootfs_supervisor /
COPY ./common/install /tmp/common/install

# Install common
RUN \
    pip3 install --no-cache-dir uv \
    && bash devcontainer_init \
    && common_install_packages \
        docker \
        shellcheck \
        cosign \
        os-agent \
        yarn \
    && usermod -aG docker vscode

COPY ./supervisor/rootfs /

# Set up venv for UV
USER vscode
ENV VIRTUAL_ENV="/home/vscode/.local/ha-venv"
RUN uv venv $VIRTUAL_ENV

# Setting PATH here isn't enough, VSCode rewites it after initial scripts finish
# Must also be set in remoteEnv in devcontainer.json
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
