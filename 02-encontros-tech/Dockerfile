FROM python:3.11-slim

# Prevent Python from writing .pyc files and buffer stdout/stderr
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PROMETHEUS_MULTIPROC_DIR=/tmp/prometheus_multiproc
ENV PORT=8000

WORKDIR /app

# Install system dependencies required for some Python packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential libpq-dev gcc \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies
COPY requirements.txt /app/requirements.txt
RUN pip install --upgrade pip setuptools wheel \
    && pip install --no-cache-dir -r /app/requirements.txt

# Copy application code
COPY . /app

# Create prometheus multiproc dir used by prometheus-flask-exporter
RUN mkdir -p ${PROMETHEUS_MULTIPROC_DIR}

EXPOSE 8000

# Start the app with Gunicorn. PORT can be overridden at runtime.
CMD ["sh", "-c", "gunicorn -w 4 -b 0.0.0.0:${PORT} --worker-class gthread --threads 2 main:app"]
