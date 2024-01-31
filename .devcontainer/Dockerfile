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
    && rm -rf /var/lib/apt/lists/*

# Add NVIDIA package repositories
RUN curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | apt-key add - \
    && curl -s -L https://nvidia.github.io/nvidia-docker/$(. /etc/os-release;echo $ID$VERSION_ID)/nvidia-docker.list | tee /etc/apt/sources.list.d/nvidia-docker.list \
    && apt-get update && apt-get install -y nvidia-docker2 nvidia-container-runtime

# Set NVIDIA as the default runtime
RUN echo '{ "default-runtime": "nvidia" }' > /etc/docker/daemon.json

# Set Poetry configuration to create virtual environments inside the project
ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_NO_INTERACTION=1



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
RUN mkdir -p /home/vscode/.config/fish && chown -R vscode:vscode /home/vscode/.config/fish && echo 'set -U fish_user_paths (poetry env info --path)/bin $fish_user_paths' >> /home/vscode/.config/fish/config.fish
RUN echo 'starship init fish | source' >> /home/vscode/.config/fish/config.fish


