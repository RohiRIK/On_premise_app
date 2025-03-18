# On-Premise Test Application

This is a simple Nginx-based application designed for testing Active Directory policies and authentication in a lab environment.

## Features

- Basic web interface showing connection information
- Displays client IP and authentication details
- Logs access and authentication attempts
- Includes network troubleshooting tools

## Prerequisites Installation

### Ubuntu/Debian
```bash
# Install Docker
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add your user to docker group (logout and login required after this)
sudo usermod -aG docker $USER

# Verify installation
docker --version
docker compose version
```

### macOS
1. Install Docker Desktop:
   - Download Docker Desktop from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
   - Double-click the downloaded `.dmg` file
   - Drag Docker to Applications folder
   - Start Docker Desktop from Applications

2. Using Homebrew (alternative method):
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Docker
brew install --cask docker

# Start Docker Desktop
open /Applications/Docker.app
```

### Windows
1. Install WSL2 (required for Docker Desktop):
```powershell
# Open PowerShell as Administrator and run:
wsl --install
```

2. Install Docker Desktop:
   - Download Docker Desktop from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
   - Run the installer (.exe)
   - During installation:
     - Enable WSL 2 features
     - Add shortcut to desktop
   - Restart your computer after installation

3. Verify Installation (in PowerShell):
```powershell
docker --version
docker compose version
```

### Troubleshooting Prerequisites

1. Docker daemon not running:
```bash
# Ubuntu
sudo systemctl start docker

# macOS/Windows
# Start Docker Desktop application
```

2. Permission issues (Ubuntu):
```bash
# If you get permission denied errors
sudo usermod -aG docker $USER
# Then log out and log back in
```

3. WSL2 issues (Windows):
```powershell
# If WSL2 installation fails, try:
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
# Then restart your computer
```

## Quick Start

1. Build and start the container:
```bash
docker-compose up -d
```

2. Access the application:
- Open your browser and navigate to `http://localhost:8080`
- The application will display connection and authentication information

3. View logs:
```bash
# View access logs
docker-compose logs -f webapp
```

## Testing AD Policies

1. The application responds on port 8080 by default
2. All requests are logged with client IP and authentication information
3. Headers show authentication status and client information
4. Built-in tools for troubleshooting:
   - `ping`
   - `nslookup`
   - `netstat`
   - `curl`

## Stopping the Application

```bash
docker-compose down
```

## Directory Structure

```
.
├── Dockerfile          # Container configuration
├── docker-compose.yml  # Service orchestration
├── nginx.conf         # Nginx server configuration
├── index.html         # Web interface
└── logs/             # Log directory (created on first run)
```

## Security Notes

- This is a test application intended for lab use only
- Do not use in production environments
- Contains basic logging and monitoring features 