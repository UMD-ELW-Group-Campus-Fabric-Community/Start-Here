# Quick Commands
This document servers as a list of common commands for interacting with the AWS EC2 instance and Docker

## Remote Connect
```bash
ssh -i [path/to/pem] ubuntu@[address]
```
## Cloning Repositories 
```bash
# Clone Databse
git clone https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Database-1.git
# Clone Website
git clone https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Website.git
```

## Configuring ~.env files
```bash
cd /path/to/env
vm [~.env file]
```

## Kill existing process
```bash
sudo lsof -i :[port]
sudo kill <pid>
```

## Basic Docker commands
```bash
# List all running containers/images
sudo docker ps
# Shutdown and remove all images, only if docker-compose.yml exist
sudo docker-compose down
# Start all images, only if docker-compose/yml exist
sudo docker-compose up [--build]
# Connect to Docker image
sudo docker exec -it [image id] bash
```
## Cleaning up your Docker mess!
```bash
# Prune EVERYTHING!
sudo docker sytsem prune
# Prune Images
sudo docker image prune
# Remove specifc Image
sudo docker rmi -a [image id]
```