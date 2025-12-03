# Base devcontainer image (Ubuntu 22.04 + common tools)
FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# ---------- OS-level dependencies ----------
USER root

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
         build-essential \
         curl \
         libgmp-dev \
         libffi-dev \
         libncurses-dev \
         xz-utils \
         zlib1g-dev \
         git \
         gnupg \
         ca-certificates \
         netbase \
    && rm -rf /var/lib/apt/lists/*

# ---------- Haskell toolchain via GHCup ----------
USER vscode
WORKDIR /workspaces

# Non-interactive GHCup bootstrap
ENV BOOTSTRAP_HASKELL_NONINTERACTIVE=1 \
    BOOTSTRAP_HASKELL_GHC_VERSION=recommended \
    BOOTSTRAP_HASKELL_CABAL_VERSION=latest \
    BOOTSTRAP_HASKELL_INSTALL_STACK=1 \
    BOOTSTRAP_HASKELL_INSTALL_HLS=1 \
    BOOTSTRAP_HASKELL_ADJUST_BASHRC=Y \
    GHCUP_USE_XDG_DIRS=1

RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh

# Make ghcup tools visible in all shells (and to VS Code)
ENV PATH="/home/vscode/.ghcup/bin:${PATH}"

# Optional: show what got installed during build
RUN ghcup --version && ghcup list

# Keep container alive for VS Code
CMD ["sleep", "infinity"]
