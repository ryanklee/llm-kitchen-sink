FROM mcr.microsoft.com/devcontainers/python:1-3.11-bullseye

ENV PYTHONUNBUFFERED 1

# Install Poetry
RUN pip install poetry

# Set the working directory in the container
WORKDIR /app

# Copy only pyproject.toml and poetry.lock (if available) to cache dependencies
COPY pyproject.toml poetry.lock* /app/

# Configure Poetry:
# - virtualenvs.create true: Create virtual environment inside the project directory
# - virtualenvs.in-project true: Create .venv directory in the project folder
RUN poetry config virtualenvs.create true \
    && poetry config virtualenvs.in-project true

# Install dependencies (including the project itself if it's a package)
RUN poetry install --no-interaction --no-ansi

# Copy the rest of your project
COPY . /app