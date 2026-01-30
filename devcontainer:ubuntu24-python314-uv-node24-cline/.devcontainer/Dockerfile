FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# =================================
# Arguments
# =================================

ARG PYTHON_VERSION=3.14

# APT source overwrite arguments
ARG OVERWRITE_APT_SOURCE=false
ARG APT_SOURCE_CONTENT=""

# =================================
# Initial setup
# =================================

USER root

SHELL ["/bin/bash", "-c"]

RUN [ ! -e /usr/bin/python ] && ln -s /usr/bin/python3 /usr/bin/python || true

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    DEBIAN_FRONTEND=noninteractive \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

# =================================
# APT sources configuration
# =================================

# 條件式執行 sources.list 更新
RUN if [[ "${OVERWRITE_APT_SOURCE}" == "true" ]]; then \
    echo "Overwriting APT source..." && \
    rm -f /etc/apt/sources.list.d/* && \
    echo "${APT_SOURCE_CONTENT}" > /etc/apt/sources.list ; \
    else \
    echo "Skipping APT source overwrite."; \
    fi

# Install system dependencies
RUN apt-get update && apt-get install -y \
    # Basic utilities
    curl \
    wget \
    git \
    vim \
    unzip \
    sudo \
    # Build dependencies
    build-essential \
    gcc \
    g++ \
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
    # Python and pip
    python3 \
    python3-pip \
    python3-venv \
    # Additional tools
    jq \
    tree

# =================================
# Node.js installation
# =================================

# Install Node.js 24.x
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | sudo bash -
RUN apt-get install -y nodejs \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install cline
RUN npm install -g cline

# =================================
# System setup
# =================================

# Add vscode to sudoers
RUN echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER vscode

# =================================
# Python environment setup
# =================================
ENV PATH="/home/vscode/.local/bin:$PATH"

# Install uv
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

# create venv using python
RUN uv venv /home/vscode/.venv --python ${PYTHON_VERSION}

# make `python` point to venv by default
ENV VIRTUAL_ENV="/home/vscode/.venv"
ENV PATH="/home/vscode/.venv/bin:${PATH}"

# ensure real pip inside venv
RUN python -m ensurepip --upgrade \
 && python -m pip install -U pip setuptools wheel

RUN ln -s /home/vscode/.venv/bin/pip3 /home/vscode/.venv/bin/pip

# Copy requirements and install Python dependencies
# COPY requirements.txt .

# Setup pip index URL if needed
ARG PIP_INDEX_URL
ARG PIP_TRUSTED_HOST
RUN if [ -n "$PIP_INDEX_URL" ]; then \
    echo "[global]" > /etc/pip.conf && \
    echo "trusted-host = $PIP_TRUSTED_HOST" >> /etc/pip.conf && \
    echo "index-url = $PIP_INDEX_URL" >> /etc/pip.conf && \
    echo "index = $PIP_INDEX_URL" >> /etc/pip.conf; \
    fi

# RUN pip install --upgrade pip && \
#     pip install --no-cache-dir -r requirements.txt

# # Switch to developer user
# USER vscode

# # Set up Python environment
# ENV PATH="/home/vscode/.local/bin:$PATH"

CMD ["sleep", "infinity"]
