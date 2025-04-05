FROM debian:bookworm

LABEL org.opencontainers.image.authors="Eneru"
LABEL org.opencontainers.image.licenses="Unlicense"
LABEL org.opencontainers.image.title="Ansible and Vagrant development"
LABEL org.opencontainers.image.description="Image that embeds every tools needed to develop on the rtl-sdr-vm project."
LABEL org.opencontainers.image.base.name="debian:bookworm"

USER root

SHELL ["/bin/sh", "-c"]

# Install packages available through apt
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
    bash-completion=1:2* \
    git=1:2.39.* \
    iproute2=6.1.* \
    jq=1.* \
    lsb-release=12.* \
    locales=2.* \
    procps=2:4.0.* \
    python3=3.11.* \
    python3-venv=3.11.* \
    pipx=1.1.* \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV PATH="$HOME/.local/bin:$PATH"

# Ensure pipx and add autocompletion
RUN pipx ensurepath && echo 'eval "$(register-python-argcomplete pipx)"' >> $HOME/.bashrc

# Install ansible, ansible auto-completions and ansible tools
RUN pipx install --include-deps ansible-dev-tools==25.4.* && \
    pipx inject --include-apps ansible-dev-tools argcomplete==3.6.* && \
    activate-global-python-argcomplete

# Install pre-commit
RUN pipx install --include-deps pre-commit

# Prepare lang environment for ansible
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8

# Set lang environment variables for ansible
ENV LANG="en_US.UTF-8" \
    LANGUAGE="en_US:en" \
    LC_ALL="en_US.UTF-8"

HEALTHCHECK --interval=1m --timeout=30s --start-period=5m --retries=3 CMD test $(ps aux | grep -i '/bin/sh -c echo Container started trap' | grep -iv 'grep' | wc -l) -ge 1