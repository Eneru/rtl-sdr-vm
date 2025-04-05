FROM debian:bookworm

LABEL org.opencontainers.image.authors="Eneru"
LABEL org.opencontainers.image.licenses="Unlicense"
LABEL org.opencontainers.image.title="Ansible and Vagrant development"
LABEL org.opencontainers.image.description="Image that embeds every tools needed to develop on the rtl-sdr-vm project."
LABEL org.opencontainers.image.base.name="debian:bookworm"

SHELL ["/bin/sh", "-c"]

# Install packages available through apt
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
    bash-completion=* \
    curl=* \
    git=* \
    gnupg2=* \
    iproute2=* \
    jq=* \
    lsb-release=* \
    locales=* \
    python3 \
    python3-dev \
    python3-venv \
    python3-pip \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

ENV PATH="$HOME/.local/bin:$PATH"

# Install pipx
RUN python3 -m pip install --break-system-packages --no-cache-dir pipx && python3 -m pipx ensurepath

# Install ansible, ansible auto-completions and ansible tools
RUN pipx install --include-deps ansible && \
    pipx inject --include-apps ansible argcomplete && activate-global-python-argcomplete && \
    pipx install ansible-dev-tools ansible-lint ansible-creator

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