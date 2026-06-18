FROM python:3.13-alpine AS builder

# install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Change the working directory to the `app` directory
WORKDIR /app

# Copy the project into the intermediate image
COPY . /app

RUN --mount=type=secret,id=nexus_username \
  --mount=type=secret,id=nexus_password \
  UV_INDEX_NEXUS_USERNAME="$(cat /run/secrets/nexus_username)" \
  UV_INDEX_NEXUS_PASSWORD="$(cat /run/secrets/nexus_password)" \
  uv sync --locked --no-editable

# ---
FROM python:3.13-alpine

# Copy the environment, but not the source code
COPY --from=builder /app/.venv /app/.venv

# Run the application
CMD ["/app/.venv/bin/study-tracker-api"]




