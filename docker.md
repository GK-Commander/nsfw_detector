# Docker Deployment Guide

## Build Image

```bash
# Build the Docker image
docker build -t nsfw_detector:latest .
```

## Push Image

```bash
# Tag the image for Docker Hub
docker tag nsfw_detector:latest chaoszhu/nsfw_detector:v1.0.0

docker tag nsfw_detector:latest chaoszhu/nsfw_detector:latest

# Login to Docker Hub
docker login

# Push to Docker Hub
docker push chaoszhu/nsfw_detector:v1.0.0
docker push chaoszhu/nsfw_detector:latest
```
