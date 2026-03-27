# Base image (official GitHub Actions runner)
FROM ghcr.io/actions/actions-runner:latest

# Switch to root for installations
USER root

# Install system dependencies (Ubuntu 24.04 compatible)
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    wget \
    gnupg \
    python3 \
    python3-venv \
    python3-pip \
    fonts-liberation \
    libnss3 \
    libxss1 \
    libasound2t64 \
    libgtk-3-0 \
    libgbm1 \
  && rm -rf /var/lib/apt/lists/*

# Install Google Chrome securely
RUN mkdir -p /etc/apt/keyrings \
  && wget -qO- https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /etc/apt/keyrings/google-chrome.gpg \
  && echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
     > /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update \
  && apt-get install -y --no-install-recommends google-chrome-stable \
  && rm -rf /var/lib/apt/lists/*

# Create Python virtual environment
RUN python3 -m venv /opt/venv

# Activate venv for all future commands
ENV PATH="/opt/venv/bin:$PATH"

# Set working directory (important for runner jobs)
WORKDIR /home/runner/work

# Copy Python requirements
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Switch back to runner user (IMPORTANT for security + ARC compatibility)
USER runner

# CRITICAL: Ensure GitHub runner lifecycle is preserved
ENTRYPOINT ["/home/runner/run.sh"]