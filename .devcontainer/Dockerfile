FROM mcr.microsoft.com/devcontainers/python:1-3.11-bullseye

ENV PYTHONUNBUFFERED 1

# Install Fish shell, curl, git, and Starship
RUN apt-get update && apt-get install -y curl git fzf fd-find bat gnupg2 software-properties-common fish && \
    && rm -rf /var/lib/apt/lists/*
RUN chsh -s /usr/bin/fish vscode
    && rm -rf /var/lib/apt/lists/*
RUN chsh -s /usr/bin/fish vscode


# Set Poetry configuration to create virtual environments inside the project
ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_NO_INTERACTION=1


ENV POETRY_VIRTUALENVS_IN_PROJECT=true \
    POETRY_NO_INTERACTION=1 \
    PATH="/workspaces/${localWorkspaceFolderBasename}/.venv/bin:$PATH"

# Install project dependencies
COPY pyproject.toml poetry.lock* /app/

# Set the PATH to use the virtual environment
RUN poetry install --no-interaction --no-root

# Copy the rest of your project
COPY . /app/

# Add poetry bin path to fish_user_paths and initiate starship
RUN mkdir -p /home/vscode/.config/fish && chown -R vscode:vscode /home/vscode/.config/fish && echo 'set -U fish_user_paths (poetry env info --path)/bin $fish_user_paths' >> /home/vscode/.config/fish/config.fish
RUN echo 'starship init fish | source' >> /home/vscode/.config/fish/config.fish


