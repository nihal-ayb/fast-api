# ─────────────────────────────────────────
# Simple FastAPI app served via Uvicorn
# ─────────────────────────────────────────
FROM python:3.12-slim AS base

LABEL maintainer="your-team@example.com"
LABEL description="Simple FastAPI application served via Uvicorn"

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Install dependencies first (better layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Run as non-root
RUN useradd --create-home appuser && chown -R appuser /app
USER appuser

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
