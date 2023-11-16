# Deploying Nginx Container in Docker and Mounting Data

## 1. Create a directory to mount data on the host machine

```shell
# Use root role to own data
cd /root

# Create Nginx directory and related files in root directory
mkdir nginx
mkdir nginx/html
mkdir nginx/conf
touch nginx/conf/nginx.conf
mkdir nginx/conf/conf.d
mkdir nginx/logs
mkdir nginx/cache
```

## 2. Deploy a simple Nginx container to get the configuration files

```shell
# Deploy a simple container
docker run -d \
--name nginx \
-p 80:80 \
-p 443:443 \
nginx

# Copy its internal configuration files
docker cp nginx:/etc/nginx/nginx.conf /root/nginx/conf
docker cp nginx:/etc/nginx/conf.d /root/nginx/conf
```

## 3. Remove the original container and redeploy

Remove the original container

```shell
docker stop nginx
docker rm nginx
```

Redeploy the Nginx container and bind data volumes at the same time

```shell
docker run -d \
--name nginx \
-p 80:80 \
-p 443:443 \
-v /root/nginx/html:/usr/share/nginx/html \
-v /root/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /root/nginx/conf/conf.d:/etc/nginx/conf.d \
-v /root/nginx/logs:/var/log/nginx  \
-v /root/nginx/cache:/var/cache/nginx \
nginx
```

## 4. Check if the deployment was successful

```shell
# Check if the Nginx container has started
docker ps -a

# Check if the data has been synchronized to the host machine directory
cd /root/nginx/conf
ls
```