# Docker

This page is a quick go-to for all the commands I usually encounter while working with docker.&#x20;

### Docker Commands

<table data-full-width="true"><thead><tr><th width="353">Description</th><th>Command</th></tr></thead><tbody><tr><td>List all docker images</td><td><code>docker images</code> or <code>docker image ls</code></td></tr><tr><td>Pull image from registry</td><td><code>docker pull &#x3C;image-name>:&#x3C;version></code></td></tr><tr><td>List all running containers</td><td><code>docker ps</code></td></tr><tr><td>List all container including stopped</td><td><code>docker ps -a</code></td></tr><tr><td>Stop running container</td><td><code>docker stop &#x3C;container-name></code></td></tr><tr><td>Remove an image</td><td><code>docker rmi &#x3C;image>:&#x3C;version></code></td></tr><tr><td>Remove container</td><td><code>docker rm &#x3C;container-name></code></td></tr><tr><td>Start an existing container</td><td><code>docker start &#x3C;container-name></code></td></tr><tr><td>Start a new container</td><td><code>docker run --name &#x3C;image>:&#x3C;version></code></td></tr><tr><td>Access terminal of running container</td><td><code>docker exec -it &#x3C;container-name> bash</code></td></tr><tr><td>Access terminal by running a container</td><td><code>docker run -it &#x3C;image>:&#x3C;version></code></td></tr><tr><td> Remove all stopped containers</td><td><code>docker container prune</code></td></tr><tr><td>Commit running container into a new image</td><td><code>docker commit &#x3C;container-id> &#x3C;image>:&#x3C;version></code></td></tr><tr><td>Inspect docker image</td><td><code>docker inspect &#x3C;image>:&#x3C;version></code></td></tr><tr><td>List docker volumes</td><td><code>docker volume ls</code></td></tr><tr><td>Create docker volume</td><td><code>docker volume create &#x3C;name></code></td></tr><tr><td>Clean unused volumes</td><td><code>docker volume prune</code></td></tr><tr><td>List networks</td><td><code>docker network ls</code></td></tr><tr><td>Create network</td><td><code>docker network create &#x3C;name></code> creates default bridge n/w</td></tr><tr><td>Attach to running console</td><td><code>docker attach &#x3C;container-name></code></td></tr></tbody></table>

#### Dockerfile

<table data-full-width="true"><thead><tr><th width="199">Instruction</th><th width="338">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>FROM</code></td><td>Used to specify a base image to start composing a docker image or a docker stage</td><td><code>FROM node:20 as ui_image</code></td></tr><tr><td><code>RUN</code></td><td>Used to run executable commands</td><td><code>RUN cp -R client /app</code></td></tr><tr><td><code>WORKDIR</code></td><td>Used to specify the directory to work on, all instructions after this command are run from within that directory</td><td><code>WORKDIR /app</code></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

#### Command Arguments for run

<table data-full-width="true"><thead><tr><th width="164">Argument</th><th width="254">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>-d</code></td><td>Run the container in detach mode</td><td><code>docker run -d &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--name</code></td><td>Give the name to container</td><td><code>docker run -d --name &#x3C;container-name> &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>-p</code></td><td>Map the host's port to the port in container</td><td><code>docker run -p 8080:80 &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>-e</code></td><td>Pass environment variables into container</td><td><code>docker run -e ENV_VAR_1=test -e ENV_VAR_2=test2</code></td></tr><tr><td><code>-it</code></td><td>Run an interactive terminal</td><td><code>docker run -it &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--rm</code></td><td>Container cleans up once stopped</td><td><code>docker run --rm -d -it &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--mount</code></td><td>To mount directories from host into the container. It's more detailed version of volume mounting</td><td><code>--mount type=bind,source=&#x3C;src>,destination=&#x3C;target>,readonly</code></td></tr><tr><td><code>-v</code></td><td>Mount volumes</td><td><code>-v app:/mnt/app</code><br><code>-v var/tmp:/mnt/app</code><br><code>-v .:/mnt/app</code></td></tr><tr><td><code>--network</code></td><td>Attach network to a container</td><td><code>docker run --network &#x3C;name> &#x3C;image>:&#x3C;version></code></td></tr><tr><td><code>--env-file</code></td><td>Load environment variables from a file</td><td><code>docker run --env-file ./.env &#x3C;image>:&#x3C;version></code></td></tr></tbody></table>

#### Command arguments for build

<table data-full-width="true"><thead><tr><th width="170">Argument</th><th width="272">Description</th><th>Example</th></tr></thead><tbody><tr><td><code>-t</code></td><td>To tag an image</td><td><code>docker build -t &#x3C;name>:&#x3C;version> .</code></td></tr><tr><td><code>-f</code></td><td>To specify different dockerfile</td><td><code>docker build -t &#x3C;name>:&#x3C;version> -f &#x3C;file-path></code></td></tr><tr><td><code>--build-arg</code></td><td>Build arguments needed in docker file</td><td><code>docker build . --build-arg NPM_TOKEN=$NPM_TOKEN WEBPACK_ENV=qa1</code></td></tr><tr><td><code>--no-cache</code></td><td>Build image from start</td><td><code>docker build . --no-cache</code></td></tr></tbody></table>



{% hint style="info" %}
Using `--rm` flag to start the container will remove all the traces of the container when stopped, meaning the stopped container will not even be visible in `docker ps -a`
{% endhint %}

{% hint style="info" %}
`-v` and `--mount` are the same, but `--mount` is more explicit and `-v` is a shorthand notation

<img src=".gitbook/assets/image (2) (1) (1).png" alt="" data-size="original">
{% endhint %}

{% hint style="info" %}
Default networks are `bridge`, `host`, `none`

* The default bridge can connect to other containers in the n/w by IP address but not by name.
* A non-default bridge network can connect to other containers in the n/w by both IP address and name.
* The host network container can connect to any other n/w containers by IP address.
{% endhint %}
