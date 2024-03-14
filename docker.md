# Docker

This page is a quick go-to for all the commands I usually encounter while working with docker.&#x20;

### Docker Commands

<table data-full-width="true"><thead><tr><th width="353">Description</th><th>Command</th></tr></thead><tbody><tr><td>List all docker images</td><td><code>docker images</code> or <code>docker image ls</code></td></tr><tr><td>Pull image from registry</td><td><code>docker pull &#x3C;image-name>:&#x3C;version></code></td></tr><tr><td>List all running containers</td><td><code>docker ps</code></td></tr><tr><td>List all container including stopped</td><td><code>docker ps -a</code></td></tr><tr><td>Stop running container</td><td><code>docker stop &#x3C;container-name></code></td></tr><tr><td>Remove an image</td><td><code>docker rmi &#x3C;image>:&#x3C;version></code></td></tr><tr><td>Remove container</td><td><code>docker rm &#x3C;container-name></code></td></tr><tr><td>Start an existing container</td><td><code>docker start &#x3C;container-name></code></td></tr><tr><td>Start a new container</td><td><code>docker run --name &#x3C;image>:&#x3C;version></code></td></tr><tr><td>Access terminal of running container</td><td><code>docker exec -it &#x3C;container-name> bash</code></td></tr><tr><td>Access terminal by running a container</td><td><code>docker run -it &#x3C;image>:&#x3C;version></code></td></tr><tr><td> Remove all stopped containers</td><td><code>docker container prune</code></td></tr><tr><td>Commit running container into a new image</td><td><code>docker commit &#x3C;container-id> &#x3C;image>:&#x3C;version></code></td></tr><tr><td>Inspect docker image</td><td><code>docker inspect &#x3C;image>:&#x3C;version></code></td></tr><tr><td>List docker volumes</td><td><code>docker volume ls</code></td></tr><tr><td>Create docker volume</td><td><code>docker volume create &#x3C;name></code></td></tr><tr><td>Clean unused volumes</td><td><code>docker volume prune</code></td></tr></tbody></table>

#### Command Arguments for run

<table data-full-width="true"><thead><tr><th width="124">Argument</th><th width="254">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>-d</code></td><td>Run the container in detach mode</td><td><code>docker run -d &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--name</code></td><td>Give the name to container</td><td><code>docker run -d --name &#x3C;container-name> &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>-p</code></td><td>Map the host's port to the port in container</td><td><code>docker run -p 8080:80 &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>-e</code></td><td>Pass environment variables into container</td><td><code>docker run -e ENV_VAR_1=test -e ENV_VAR_2=test2</code></td></tr><tr><td><code>-it</code></td><td>Run an interactive terminal</td><td><code>docker run -it &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--rm</code></td><td>Container cleans up once stopped</td><td><code>docker run --rm -d -it &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--mount</code></td><td>To mount directories from host into the container. It's more detailed version of volume mounting</td><td><code>--mount type=bind,source=&#x3C;src>,destination=&#x3C;target>,readonly</code></td></tr><tr><td><code>-v</code></td><td>Mount volumes</td><td><code>-v app:/mnt/app</code><br><code>-v var/tmp:/mnt/app</code><br><code>-v .:/mnt/app</code></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>



{% hint style="info" %}
Using `--rm` flag to start the container will remove all the traces of the container when stopped, meaning the stopped container will not even be visible in `docker ps -a`
{% endhint %}

{% hint style="info" %}
`-v` and `--mount` are the same, but `--mount` is more explicit and `-v` is a shorthand notation

<img src=".gitbook/assets/image (2).png" alt="" data-size="original">
{% endhint %}

