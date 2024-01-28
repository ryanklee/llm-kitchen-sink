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

# Copy the rest of your project
COPY . /app

# Add poetry environment path to fish shell configuration
RUN echo 'set -gx PATH $PATH /workspaces/llm-kitchen-sink/.venv/bin' >> ~/.config/fish/config.fish
