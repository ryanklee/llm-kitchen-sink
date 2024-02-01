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
ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_NO_INTERACTION=1



# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 - && \
    echo 'export PATH="/root/.local/bin:$PATH"' >> $HOME/.bashrc && \
    /root/.local/bin/poetry config virtualenvs.create false

# Set Poetry configuration to create virtual environments inside the project
ENV POETRY_NO_INTERACTION=1 \
    POETRY_CACHE_DIR=/var/cache/pypoetry

# Install project dependencies
COPY pyproject.toml poetry.lock ./

# Set the PATH to use the virtual environment
ENV PATH="/workspaces/${localWorkspaceFolderBasename}/.venv/bin:$PATH"

# Copy the rest of your project
COPY . /app

# Add poetry bin path to fish_user_paths and initiate starship
RUN mkdir -p /home/vscode/.config/fish && chown -R vscode:vscode /home/vscode/.config/fish
RUN echo 'for file in ~/.config/fish/conf.d/*.fish; source $file; end' > /home/vscode/.config/fish/config.fish
RUN mkdir -p /home/vscode/.config/fish/conf.d
RUN echo 'starship init fish | source' > /home/vscode/.config/fish/conf.d/starship.fish
RUN chown -R vscode:vscode /home/vscode/.config/fish/conf.d/starship.fish
# Use the installed Poetry to set up the environment
ENV PATH="/root/.local/bin:$PATH"
RUN poetry env use python3.11
RUN poetry install --no-interaction --no-root -n $(nproc)
RUN echo 'set -U fish_user_paths /workspaces/${localWorkspaceFolderBasename}/.venv/bin $fish_user_paths' >> /home/vscode/.config/fish/config.fish
