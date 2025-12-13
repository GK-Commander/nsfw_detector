# Docker Deployment Guide

## Build Image

```bash
# Build the Docker image
docker build -t nsfw_detector:latest .

# Build with a specific tag
docker build -t nsfw_detector:v1.0.0 .
```

## Push Image

```bash
# Tag the image for Docker Hub
docker tag nsfw_detector:latest your-username/nsfw_detector:latest

# Login to Docker Hub
docker login

# Push to Docker Hub
docker push your-username/nsfw_detector:latest
```

## Run Container

### Option 1: Docker Run

**Basic usage (without authentication):**

```bash
docker run -d -p 3333:3333 --name nsfw-detector nsfw_detector:latest
```

**With custom configuration (enable token authentication):**

```bash
# Create a config file first
echo "auth_token = your_secret_token" > /path/to/config

# Run with config mounted
docker run -d -p 3333:3333 \
  -v /path/to/config:/tmp/config \
  --name nsfw-detector \
  nsfw_detector:latest
```

**With local file detection (mount local directory):**

```bash
docker run -d -p 3333:3333 \
  -v /path/to/files:/path/to/files \
  -v /path/to/config:/tmp/config \
  --name nsfw-detector \
  nsfw_detector:latest
```

### Option 2: Docker Compose

**1. Create `docker-compose.yml`:**

```yaml
version: '3.8'

services:
  nsfw-detector:
    image: nsfw_detector:latest
    container_name: nsfw-detector
    restart: unless-stopped
    ports:
      - "3333:3333"
    volumes:
      # Mount config file (optional, for custom settings like auth_token)
      - ./config:/tmp/config
      # Mount local directory for file detection (optional)
      # - /path/to/files:/path/to/files
    environment:
      - TZ=Asia/Shanghai
```

**2. Create `config` file (optional):**

```ini
# NSFW detection threshold (0-1, higher = stricter)
nsfw_threshold = 0.95

# Maximum frames to process for video
ffmpeg_max_frames = 30

# Video processing timeout (seconds)
ffmpeg_max_timeout = 1800

# API authentication token (optional)
# Uncomment the line below to enable authentication
# auth_token = your_secret_token
```

**3. Start the service:**

```bash
# Start in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the service
docker-compose down
```

## Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| `nsfw_threshold` | `0.8` | NSFW detection threshold (0-1) |
| `ffmpeg_max_frames` | `20` | Max frames to extract from video |
| `ffmpeg_max_timeout` | `1800` | Video processing timeout (seconds) |
| `auth_token` | `None` | API authentication token (optional) |

## API Usage

**Without authentication:**

```bash
curl -X POST -F "file=@/path/to/image.jpg" http://localhost:3333/check
```

**With authentication:**

```bash
curl -X POST \
  -H "Authorization: Bearer your_secret_token" \
  -F "file=@/path/to/image.jpg" \
  http://localhost:3333/check
```

**Check local file (requires volume mount):**

```bash
curl -X POST -F "path=/path/to/image.jpg" http://localhost:3333/check
```

## Web Interface

Open your browser and visit: [http://localhost:3333](http://localhost:3333)

If authentication is enabled, enter your token in the input field before uploading files.
