# Use Python 3.10 slim image — matches local dev (3.8+) and keeps image small
FROM public.ecr.aws/docker/library/python:3.10-slim-bookworm

# Set working directory inside container
WORKDIR /app

# Install OS-level build deps required by psycopg2 to compile/link against libpq
# build-essential: gcc + make; libpq-dev: PostgreSQL client lib headers
# --no-install-recommends keeps image lean; cleanup removes apt cache to save space
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends build-essential libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Copy requirements first to leverage Docker layer caching (deps re-install only if reqs change)
COPY requirements.txt .

# Upgrade pip toolchain, then install Python deps
RUN pip install --upgrade pip setuptools wheel && \
    pip install --no-cache-dir -r requirements.txt

# Copy application source
COPY . .

# Document the port the Flask app listens on (informational; doesn't publish)
EXPOSE 5153

# Run the Flask app
CMD ["python", "app.py"]
