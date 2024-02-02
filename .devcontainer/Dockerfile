FROM mcr.microsoft.com/devcontainers/python:1-3.11-bullseye

ENV PYTHONUNBUFFERED 1

# Install Fish shell, curl, git, and Starship
RUN apt-get update && apt-get install -y \
        curl \
        git \
        fzf \
        fd-find \
        bat \
        gnupg2 \
        software-properties-common \
        fish \
    && rm -rf /var/lib/apt/lists/*

# Set Fish as the default shell for the vscode user
RUN chsh -s /usr/bin/fish vscode

RUN curl -sSL https://install.python-poetry.org | python3 - && \
    mkdir -p /home/vscode/.local/bin && \
    chown -R vscode:vscode /home/vscode/.local && \
    ln -s /root/.local/bin/poetry /home/vscode/.local/bin/poetry && \
    echo 'set -U fish_user_paths /home/vscode/.local/bin $fish_user_paths' | sudo -u vscode fish




# Install project dependencies
COPY pyproject.toml poetry.lock ./

# Set the PATH to use the virtual environment
RUN mkdir -p /home/vscode/.config/fish && chown -R vscode:vscode /home/vscode/.config && \
    echo 'set -U fish_user_paths /workspaces/'"$localWorkspaceFolderBasename"'/.venv/bin $fish_user_paths' >> /home/vscode/.config/fish/config.fish && \
    chown -R vscode:vscode /home/vscode/.config

RUN curl -sL https://git.io/fisher | source && fisher install jorgebucaran/fisher

RUN sh -c "$(curl -fsSL https://starship.rs/install.sh)" -- --yes

# Copy the rest of your project
COPY . /app

