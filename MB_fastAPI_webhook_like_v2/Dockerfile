FROM python:3.11-slim

WORKDIR /app

# Install system dependencies including build essentials for numpy
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    wget \
    iputils-ping \
    netcat-openbsd \
    procps \
    libffi-dev \
    ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create necessary directories
RUN mkdir -p /app/captcha_image /app/data

# Copy requirements first to leverage Docker cache
COPY requirements.txt .

# Create a modified requirements file without the problematic torch and torchvision versions
RUN grep -v "torch==" requirements.txt | grep -v "torchvision==" > requirements_modified.txt

# Install setuptools and wheel first, then install dependencies
RUN pip install --no-cache-dir --upgrade pip setuptools wheel && \
    pip install --no-cache-dir -r requirements_modified.txt && \
    pip install --no-cache-dir requests[security] urllib3

# Install PyTorch and torchvision with CPU support without version constraints
RUN pip install --no-cache-dir torch torchvision --extra-index-url https://download.pytorch.org/whl/cpu

# Add a health endpoint for health checks
COPY . .

# Create a health endpoint file
RUN echo 'from fastapi import FastAPI, Depends\nfrom main import app\n\n@app.get("/health")\ndef health_check():\n    return {"status": "ok", "service": "mb-fastapi"}' > health_endpoint.py

# Fix file permissions
RUN chmod -R 755 /app

# Add a fix script for Mac networking issues
RUN echo '#!/bin/bash\n\
# Wait for selenium hub to be ready\n\
echo "Waiting for Selenium Hub..."\n\
timeout=60\n\
while [ $timeout -gt 0 ] && ! nc -z selenium-hub 4444; do\n\
    echo "Waiting for Selenium Hub ($timeout seconds left)..."\n\
    sleep 5\n\
    timeout=$((timeout-5))\n\
done\n\
\n\
if [ $timeout -le 0 ]; then\n\
    echo "Selenium Hub not available, but continuing anyway"\n\
else\n\
    echo "Selenium Hub is ready!"\n\
fi\n\
\n\
# Run the actual startup script\n\
exec python3 -u startup.py\n' > /app/docker-entrypoint.sh && chmod +x /app/docker-entrypoint.sh

# Use the entrypoint script that handles Mac-specific issues
ENTRYPOINT ["/app/docker-entrypoint.sh"]