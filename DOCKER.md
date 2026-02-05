# Docker Setup for Avataaars Generator

This project includes Docker configuration for containerized deployment and GitHub Actions for automated building and pushing to GitHub Container Registry (GHCR).

## Building Locally

### Build the Docker image:
```bash
docker build -t avataaars-generator:latest .
```

### Run the container:
```bash
docker run -p 80:80 avataaars-generator:latest
```

The application will be available at `http://localhost`

### Build with specific tag:
```bash
docker build -t ghcr.io/YOUR_USERNAME/avataaars-generator:latest .
```

## GitHub Actions Automation

The GitHub Actions workflow (`.github/workflows/docker-build.yml`) automatically:

1. **Triggers on:**
   - Push to `master` or `main` branch
   - Creation of version tags (e.g., `v1.0.0`)
   - Pull requests to `master` or `main` (builds but doesn't push)

2. **Tags the image with:**
   - Branch name (e.g., `ghcr.io/owner/repo:master`)
   - Semantic version (for tags like `v1.0.0`)
   - Latest tag (for main/master branch)
   - Short SHA commit hash

3. **Pushes to:**
   - GitHub Container Registry (GHCR) at `ghcr.io/OWNER/REPO`

## Setup Requirements

The GitHub Actions workflow uses `secrets.GITHUB_TOKEN` which is automatically created. No additional setup is required.

### Container Registry Access

Your image will be pushed to: `ghcr.io/YOUR_GITHUB_USERNAME/avataaars-generator`

To pull the image:
```bash
docker pull ghcr.io/YOUR_GITHUB_USERNAME/avataaars-generator:latest
```

## Docker Compose (Optional)

Create a `docker-compose.yml` for local development if needed:

```yaml
version: '3.8'

services:
  avataaars:
    build: .
    ports:
      - "80:80"
    environment:
      - NODE_ENV=production
```

Then run:
```bash
docker-compose up
```

## Notes

- The Dockerfile uses a multi-stage build to optimize image size
- Nginx is used as the production-grade web server
- The image includes health checks for monitoring
- Cache layers are configured for faster rebuilds
