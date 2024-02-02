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



# Set Poetry configuration to create virtual environments inside the project
ENV POETRY_NO_INTERACTION=1 \
    POETRY_CACHE_DIR=/var/cache/pypoetry

# Install project dependencies
COPY pyproject.toml poetry.lock ./

# Set the PATH to use the virtual environment
RUN mkdir -p /home/vscode/.config/fish && chown -R vscode:vscode /home/vscode/.config

# Copy the rest of your project
COPY . /app

