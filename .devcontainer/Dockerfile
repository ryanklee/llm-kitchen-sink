FROM mcr.microsoft.com/devcontainers/python:1-3.11-bullseye

ENV PYTHONUNBUFFERED 1

# Assuming the default user is `vscode`
ENV USER_HOME_DIR /home/vscode

WORKDIR /app

# Install Fish shell, curl, git, and Starship
RUN apt-get update && apt-get install -y \
        fish \
        curl \
        git \
    && rm -rf /var/lib/apt/lists/* \
    && curl -sS https://starship.rs/install.sh | sh -s -- -y \
    && curl https://ollama.ai/install.sh | sh

# Copy pyproject.toml (if available) to cache dependencies
COPY pyproject.toml* /app/

# Install Poetry, configure it, and ensure the correct Python version is used
RUN pip install poetry \
    && poetry config virtualenvs.create true \
    && poetry config virtualenvs.in-project true \
    && poetry env use python3.11

# Install Fisher and plugins using a Fish shell script
RUN echo 'curl -sL https://git.io/fisher | source; \
           fisher install jorgebucaran/fisher; \
           fisher install jethrokuan/z; \
           fisher install PatrickF1/fzf.fish; \
           fisher install meaningful-ooo/sponge;' \
    | fish

# Initialize Starship in Fish shell for the vscode user
RUN mkdir -p $USER_HOME_DIR/.config/fish/conf.d \
    # Create a default config.fish file
    && echo '# Fish shell configuration file' > $USER_HOME_DIR/.config/fish/config.fish \
    && echo '' >> $USER_HOME_DIR/.config/fish/config.fish \
    # Append Starship initialization
    && echo 'starship init fish | source' >> $USER_HOME_DIR/.config/fish/config.fish \
    # Fish shell configuration to automatically activate the Poetry environment
    && printf 'if test -f /app/poetry.lock\n    and command -q poetry\n    source (poetry env info --path)/bin/activate.fish\nend\n' > $USER_HOME_DIR/.config/fish/conf.d/poetry.fish \
    && chown -R vscode:vscode $USER_HOME_DIR/.config
# Copy pyproject.toml and poetry.lock (if available) to cache dependencies
COPY pyproject.toml poetry.lock* /app/

# Install dependencies
RUN poetry install --no-interaction --no-ansi

# Copy the rest of your project
COPY . /app

# Set Fish as the default shell
SHELL ["/usr/bin/fish", "-c"]
