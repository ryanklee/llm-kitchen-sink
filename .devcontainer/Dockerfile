FROM mcr.microsoft.com/devcontainers/python:1-3.11-bullseye

ENV PYTHONUNBUFFERED 1

# Install Fish shell, curl, git, and Starship
RUN apt-get update && apt-get install -y \
        curl \
        git \
        fzf \
        fd-find \
        bat \
    && rm -rf /var/lib/apt/lists/*

# Set Poetry configuration to create virtual environments inside the project
ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_NO_INTERACTION=1

# Install project dependencies
COPY pyproject.toml poetry.lock ./

# Set the PATH to use the virtual environment
ENV PATH="/workspaces/${localWorkspaceFolderBasename}/.venv/bin:$PATH"

# Copy the rest of your project
COPY . /app

# Add poetry bin path to fish_user_paths and initiate starship
RUN mkdir -p ~/.config/fish && echo 'set -U fish_user_paths (poetry env info --path)/bin $fish_user_paths' >> ~/.config/fish/config.fish
RUN echo 'starship init fish | source' >> ~/.config/fish/config.fish


