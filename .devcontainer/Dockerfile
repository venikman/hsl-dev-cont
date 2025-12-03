# Base devcontainer image (Ubuntu LTS + common tooling)
# https://mcr.microsoft.com/en-us/product/devcontainers/base/about
FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# ---------- OS-level deps (as root) ----------
USER root

# Basic build + libraries required by GHCup / GHC / Stack
# https://www.haskell.org/ghcup/install/  and  https://docs.haskellstack.org/en/stable/install_and_upgrade/
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

# ---------- Haskell toolchain via GHCup (as 'vscode' user) ----------
# https://www.haskell.org/ghcup/  and non-interactive envs :contentReference[oaicite:1]{index=1}
USER vscode
WORKDIR /workspaces

# Non-interactive bootstrap: install ghcup + recommended GHC + cabal + Stack + HLS
ENV BOOTSTRAP_HASKELL_NONINTERACTIVE=1 \
    BOOTSTRAP_HASKELL_GHC_VERSION=recommended \
    BOOTSTRAP_HASKELL_CABAL_VERSION=latest \
    BOOTSTRAP_HASKELL_INSTALL_STACK=1 \
    BOOTSTRAP_HASKELL_INSTALL_HLS=1 \
    BOOTSTRAP_HASKELL_ADJUST_BASHRC=Y \
    GHCUP_USE_XDG_DIRS=1

RUN curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh

# Make ghcup tools visible to VS Code shells
ENV PATH="/home/vscode/.ghcup/bin:${PATH}"

# Optional: show what got installed at build time
RUN ghcup --version && ghcup list

# Keep container alive for VS Code
CMD ["sleep", "infinity"]
