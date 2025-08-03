FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# Install system dependencies as root
RUN apt-get update && \
    apt-get install -y curl git && \
    rm -rf /var/lib/apt/lists/*

# Switch to vscode user for Lean/elan/lake install
USER vscode
ENV PATH="/home/vscode/.elan/bin:${PATH}"

# Install elan (Lean version manager) and Lean 4 as vscode user
RUN curl https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh -sSf | sh -s -- -y

# Add Lean 4 to PATH for all users and future sessions
RUN echo 'export PATH="$HOME/.elan/bin:$PATH"' | sudo tee -a /etc/profile.d/lean.sh > /dev/null

# Pre-install Lean toolchain and lake, and update mathlib
RUN /home/vscode/.elan/bin/lean --version && \
    /home/vscode/.elan/bin/lake --version

COPY ./lakefile.toml /tmp/lakefile.toml
RUN cd /tmp && /home/vscode/.elan/bin/lake update
