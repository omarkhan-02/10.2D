# product-service/Dockerfile
# order-service/Dockerfile
# customer-service/Dockerfile

# STAGE 1: Build stage to install dependencies using Poetry
FROM python:3.9-slim as builder

WORKDIR /app

# Install Poetry package manager
RUN pip install poetry

# Copy only the dependency configuration files to leverage Docker caching
COPY ../poetry.lock ../pyproject.toml ./

# Install project dependencies into a non-activated virtual environment
RUN poetry config virtualenvs.create false && \
    poetry install --no-root --no-dev

# STAGE 2: Final application image
FROM python:3.9-slim

WORKDIR /app

# Copy the installed Python packages from the builder stage
COPY --from=builder /usr/local/lib/python3.9/site-packages /usr/local/lib/python3.9/site-packages

# Copy the application source code for this specific service
COPY . .

# Expose the port that the Gunicorn server will run on
EXPOSE 8000

# Run the application using Gunicorn with Uvicorn workers for production
CMD ["gunicorn", "-k", "uvicorn.workers.UvicornWorker", "-w", "4", "-b", "0.0.0.0:8000", "main:app"]