# CLI

## Image

```bash
# List available images
docker images

# Pull image from container registry (default from Dockerhub)
docker pull alpine:3.7

# Build image with custom Dockerfile
docker build -t my-first-image:0.1 -f Dockerfile .

# List layer information of a image 
docker history my-first-image:0.1

# Tag a image
docker tag my-first-image:0.1 tachingchen/my-first-image:0.1

# Push image to remote container registry
docker push tachingchen/my-first-image:0.1

# Remove a image or untagged it
docker rmi my-first-image:0.1
```

## Container

```bash
# Create & start running a container
docker run --rm -it tachingchen/my-first-image:0.1

# Create a container
docker create alpine:3.7 ping 8.8.8.8

# Start a container
docker start 03e681d4ee83

# Pause a container
docker pause 03e681d4ee83

# Unpause a container
docker unpause 03e681d4ee83

# Stop a container
docker stop 03e681d4ee83

# Restart a container
docker restart 03e681d4ee83

# Force to kill a container
docker kill 03e681d4ee83

# Remove a container
docker rm 03e681d4ee83
```

## Monitoring

```bash
# Show detail information of a container
docker inspect faa65c41bd7e

# Show resource usage
docker stats

# Show running containers. Add "-a" to show stopped containers.
docker ps 

# Show top information of a container
docker top faa65c41bd7e

# Print logs of a container 
docker logs faa65c41bd7e
```

## Misc

```bash
# Copy local file to given path of a container
docker cp ./ha.sh f2b6fe75548d:/tmp/ha.sh

# Copy ile from a container to given local path
docker cp f2b6fe75548d:/tmp/lol.txt . 

# Execute command inside a container
docker exec f2b6fe75548d sh /tmp/ha.sh

# Add "--it" flag to attach container shell
docker exec -it f2b6fe75548d sh 
```