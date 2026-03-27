# docker/selenium-runner/Dockerfile
FROM ghcr.io/actions/actions-runner:latest
USER root

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
    libasound2 \
    libgtk-3-0 \
    libgbm1 \
  && rm -rf /var/lib/apt/lists/*

# Install Google Chrome (APT)
RUN wget -qO- https://dl.google.com/linux/linux_signing_key.pub | gpg --dearmor -o /usr/share/keyrings/google-chrome.gpg \
  && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main" \
        > /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update && apt-get install -y --no-install-recommends google-chrome-stable \
  && rm -rf /var/lib/apt/lists/*

# Python venv and wheelhouse
RUN python3 -m venv /opt/venv 
ENV PATH="/opt/venv/bin:$PATH"

# Create working dir & add minimal scripts
WORKDIR /workspace
COPY requirements.txt /workspace/requirements.txt
# Install test deps into venv (this is done at build time; if builder has internet)
RUN /opt/venv/bin/pip install --no-cache-dir -r /workspace/requirements.txt

USER runner