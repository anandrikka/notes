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
<strong># d: Run container in detach mode
</strong><strong># p: Map ports
</strong><strong># v: Map volumes
</strong><strong># --name: Name of the container
</strong># docker run --name nginx_demo -d -p 8080:80 -v $(pwd):/usr/share/nginx/html nginx:latest
docker run --name &#x3C;container-name> -d -p &#x3C;host-port>:&#x3C;container-port> -v &#x3C;local-path>:&#x3C;container-path> &#x3C;image>:&#x3C;version>

# bash into the image
docker run -it nginx:latest bash

# bash into a container
docker exec -it &#x3C;container-name> bash

# Create an image from the container
# docker commit abch12lddss custom-postgres:v1
docker commit &#x3C;container-id> &#x3C;name-of-image>:&#x3C;version>

# Inspect docker image
docker inspect &#x3C;image-name>:&#x3C;version>
# Can use the -f flag to extract specifics from a json
docker inspect &#x3C;image-name>:&#x3C;version> -f '{{ json .ContainerConfig }}'

# Add Environment variables
docker run --name &#x3C;container-name> -e &#x3C;env-name>=&#x3C;value> -e &#x3C;env-name>=&#x3C;value> &#x3C;image-name>:&#x3C;version>

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
</strong></code></pre>

{% hint style="info" %}
Using `--rm` flag to start the container will remove all the traces of the container when stopped, meaning the stopped container will not be even visible in `docker ps -a`
{% endhint %}

{% hint style="info" %}
`-v` and `--mount` are the same, but `--mount` is more explicit and `-v` is a shorthand notation

<img src=".gitbook/assets/image (2).png" alt="" data-size="original">
{% endhint %}

