FROM python:3.10-slim AS build-env

ARG POETRY_VERSION=1.3.2

# Copy files
COPY . /app
WORKDIR /app

# Install production dependencies
RUN pip install --upgrade pip && \
    pip install poetry==$POETRY_VERSION && \
    poetry config virtualenvs.in-project true && \
    poetry install --without dev --no-interaction --no-ansi

# Runtime
FROM python:3.10-slim

ARG PORT=8000
ENV PORT=${PORT}

COPY --from=build-env /app /app
WORKDIR /app

CMD ["/bin/sh", "-c", "/app/.venv/bin/hypercorn conduit:app --bind 0.0.0.0:${PORT} --debug --access-logfile - --error-logfile -"]
