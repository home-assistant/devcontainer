FROM mcr.microsoft.com/devcontainers/python:3-3.14

ENV \
    DEBIAN_FRONTEND=noninteractive \
    DEVCONTAINER=1 \
    NVM_DIR="/root/.nvm"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Uninstall pre-installed formatting and linting tools
# They would conflict with our pinned versions
RUN \
    pipx uninstall pydocstyle \
    && pipx uninstall pycodestyle \
    && pipx uninstall mypy \
    && pipx uninstall pylint

# Install tools
RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        dbus-broker \
        network-manager \
        libpulse0 \
        systemd-journal-remote \
        systemd-resolved \
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
    && usermod -aG docker vscode

COPY ./supervisor/rootfs /

# Set up venv for UV
USER vscode
ENV VIRTUAL_ENV="/home/vscode/.local/ha-venv"
RUN uv venv $VIRTUAL_ENV

# Setting PATH here isn't enough, VSCode rewites it after initial scripts finish
# Must also be set in remoteEnv in devcontainer.json
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# Configure systemd as init system
USER root

# Remove the fake systemctl and policy-rc.d that the base image ships
# to prevent service management during build. We need the real systemctl
# since this container runs systemd as PID 1.
RUN rm -f /usr/local/bin/systemctl /usr/sbin/policy-rc.d

# Mask unnecessary systemd units to make sure they don't interfere
RUN systemctl mask \
    systemd-networkd.service \
    systemd-networkd-wait-online.service \
    getty@.service \
    serial-getty@.service

# Create persistent journal directory so journald stores logs on disk
RUN systemd-tmpfiles --create --prefix /var/log/journal

# Enable services which are otherwise disabled by default
RUN systemctl enable \
    haos-agent \
    systemd-journal-gatewayd

STOPSIGNAL SIGRTMIN+3
CMD ["/sbin/init"]
