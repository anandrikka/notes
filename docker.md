# Docker

This page is a quick go-to for all the commands I usually encounter while working with docker.&#x20;

<pre class="language-shell" data-title="Docker Commands" data-overflow="wrap" data-full-width="true"><code class="lang-shell">############### Most commonly used ##################

# List All docker images
docker image ls

# Pull the image from the docker hub
# If the version is not specified, it will be the latest by default
docker pull &#x3C;image-name>:&#x3C;version>

# List All running containers
docker ps

# List All containers including stopped
docker ps -a

# Stop a running container
docker stop &#x3C;container-name>

# Remove an image
docker rmi &#x3C;image-name>:&#x3C;version>

# Remove container
docker rm &#x3C;container-name>

# Run a docker container from an image
# Arguments accepted to run the image
# -d        To detach the container from the terminal, so it doesn't block it
# -p        Map the host's port to the port in the container. Eg: -p 8081:80
# -v        Map the docker volume or directory with the container directory
#           Ex: -v app:/mnt/app mount the docker volume `app` to container `/mnt/app`
#           Ex: -v ./var/tmp:/mnt/app mount the folder `/var/tmp` to `/mnt/app`
#           Ex: -v .:/mnt/tmp mount current folder to /mnt/tmp
# -e        Environment variable into the container. Eg: -e POSTGRES_PASSWORD='demo$'
# -it       To delve into the container terminal. Eg: docker run -it &#x3C;container-name> bash
# --mount   To mount the directory from the host into the container, it's the more detailed version of volume binding
#           Ex: --mount type=bind,source=&#x3C;src-directory>,destination=&#x3C;target-directory>,readonly     
docker run --name &#x3C;container-name> &#x3C;image>:&#x3C;version>

# bash into a container
docker exec -it &#x3C;container-name> bash

# Create an image from the container
# docker commit abch12lddss custom-postgres:v1
docker commit &#x3C;container-id> &#x3C;name-of-image>:&#x3C;version>

# Inspect docker image
docker inspect &#x3C;image-name>:&#x3C;version>
# Can use the -f flag to extract specifics from a json
docker inspect &#x3C;image-name>:&#x3C;version> -f '{{ json .ContainerConfig }}'

# Start the stopped container
docker start &#x3C;container-name>

################## Volumes ########################

# Check the list of volumes
docker volume ls

# Create a volume
docker volume create &#x3C;name>

# Remove unused volumes
docker volume prune

# Attaching volume to a container
# We can attach multiple volumes, volumes are mapped separated by a :
# The left side is from the host and the right side is in the container
docker run --name nginx -v $(pwd):/usr/share/nginx -v /tmp/data:data

# Mount directory from host into the container
<strong>docker run -d --name &#x3C;container-name> --mount type=bind,source=&#x3C;src-directory>,destination=&#x3C;target-directory>,readonly &#x3C;image-name>:&#x3C;version>
</strong><strong>
</strong><strong>################## Networks ########################
</strong><strong>
</strong></code></pre>

{% hint style="info" %}
Using `--rm` flag to start the container will remove all the traces of the container when stopped, meaning the stopped container will not be even visible in `docker ps -a`
{% endhint %}

{% hint style="info" %}
`-v` and `--mount` are the same, but `--mount` is more explicit and `-v` is a shorthand notation

<img src=".gitbook/assets/image (2).png" alt="" data-size="original">
{% endhint %}

