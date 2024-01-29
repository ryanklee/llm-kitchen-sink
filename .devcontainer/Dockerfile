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

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python -

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python -

# Set Poetry configuration to create virtual environments inside the project
ENV POETRY_VIRTUALENVS_IN_PROJECT=true
ENV POETRY_NO_INTERACTION=1

# Install project dependencies
COPY pyproject.toml poetry.lock ./
RUN poetry install --no-interaction

# Set the PATH to use the virtual environment
ENV PATH="/workspaces/${localWorkspaceFolderBasename}/.venv/bin:$PATH"

# Copy the rest of your project
COPY . /app


