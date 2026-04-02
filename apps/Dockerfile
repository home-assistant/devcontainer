FROM mcr.microsoft.com/devcontainers/base:2-debian

ENV \
    DEBIAN_FRONTEND=noninteractive \
    DEVCONTAINER=1

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

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
    bash devcontainer_init \
    && common_install_packages \
        docker \
        shellcheck \
        cosign \
        os-agent \
    && usermod -aG docker vscode

COPY ./apps/rootfs /

# Configure systemd as init system
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
