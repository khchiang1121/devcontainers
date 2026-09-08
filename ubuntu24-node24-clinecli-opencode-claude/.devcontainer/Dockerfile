FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# =================================
# Arguments
# =================================

ARG OVERWRITE_APT_SOURCE=true
ARG APT_SOURCE_CONTENT="\
Types: deb \n\
URIs: http://archive.ubuntu.com/ubuntu/ \n\
Suites: noble noble-updates noble-backports \n\
Components: main universe restricted multiverse \n\
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg \n\
\n\
Types: deb \n\
URIs: http://security.ubuntu.com/ubuntu/ \n\
Suites: noble-security \n\
Components: main universe restricted multiverse \n\
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg \n\
\n\
Types: deb \n\
URIs: http://ppa.launchpad.net/apt-fast/stable/ubuntu \n\
Suites: noble \n\
Components: main \n\
Trusted: yes \n\
\n\
Types: deb \n\
URIs: https://packages.microsoft.com/repos/vscode \n\
Suites: stable \n\
Components: main \n\
Trusted: yes \n\
\n\
Types: deb \n\
URIs: https://packages.buildkite.com/helm-linux/helm-debian/any/ \n\
Suites: any \n\
Components: main \n\
Trusted: yes \n\
"

# =================================
# Initial setup
# =================================

USER root

SHELL ["/bin/bash", "-c"]

RUN [ ! -e /usr/bin/python ] && ln -s /usr/bin/python3 /usr/bin/python || true

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive

    # =================================
# APT sources configuration
# =================================
RUN if [[ "${OVERWRITE_APT_SOURCE}" == "true" ]]; then \
    echo "Overwriting APT source..." && \
    rm -f /etc/apt/sources.list.d/* && \
    printf "${APT_SOURCE_CONTENT}" > /etc/apt/sources.list.d/custom.sources ; \
    else \
    echo "Skipping APT source overwrite."; \
    fi

# Install system dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    # Basic utilities
    curl \
    wget \
    git \
    vim \
    unzip \
    sudo \
    # Build dependencies
    build-essential \
    make \
    # PostgreSQL client
    postgresql-client \
    libpq-dev \
    # Network utilities
    net-tools \
    iputils-ping \
    netcat-openbsd \
    dnsutils \
    # Process utilities
    htop \
    procps \
    # SSL certificates
    ca-certificates \
    # Additional tools
    jq \
    tree \
    # Chrome installation for cline
    && wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb \
    && apt-get install -y ./google-chrome-stable_current_amd64.deb \
    && rm google-chrome-stable_current_amd64.deb \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# =================================
# Node.js installation
# =================================

# Install Node.js 24.x
# Install cline, claude-code and claude-code-router

RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs \
    && npm install -g cline @anthropic-ai/claude-code @musistudio/claude-code-router \
    && npm cache clean --force \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# =================================
# System setup
# =================================

# Add vscode to sudoers
RUN echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER vscode

# =================================
# OpenCode installation
# =================================
RUN curl -fsSL https://opencode.ai/install | bash
ENV PATH="/home/vscode/.opencode/bin:${PATH}"

# =================================
# Bun installation
# =================================
RUN curl -fsSL https://bun.com/install | bash
ENV PATH="/home/vscode/.bun/bin:${PATH}"

# =================================
# OpenCode Initialize
# =================================
RUN opencode run "test"

# =================================
# oh-my-opencode installation
# =================================
# RUN VERSION=$(npm view oh-my-openagent version) \
#     && echo "Installing oh-my-openagent version: ${VERSION}" \
#     && npx -y oh-my-openagent@${VERSION} install --no-tui --claude=no --gemini=no --copilot=no --openai=no --opencode-zen=no --zai-coding-plan=no \
#     && sed -i "s/oh-my-openagent@latest/oh-my-openagent@${VERSION}/" ~/.config/opencode/opencode.jsonc \
#     && rm -rf ~/.npm/_npx

CMD ["sleep", "infinity"]
