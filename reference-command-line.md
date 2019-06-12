# reference-command-line

A handy list of command line terminal, well, commands, that I will probably need in the future.

---

## General

* `cd [directory]` Change the working directory.
* `cd ..` Exit one directory up from current directory.
* `ls` List directory contents.
* `ls -l` List directory contents, use a long listing format.
* `ls -R` List all directory and sub-directory contents.
* `ssh` OpenSSH SSH client (remote login program).
* `-i identity_file` Selects a file from which the identity (private key) for public key authentication is read.
* `username@public-ip-address` SSH connects and logs into the specified hostname (with optional user name).
* `mkdir [directory-name]` Make a directory folder.
* `nano [file-name]` Make a file using nano text editor.
* `cat [file-name]` Prints file contents to terminal.
* `mount [directory-to-mount] [directory-to-be-mounted-on]` Mount a directory onto another.
* `umount [directory]` Unmount directory.

---

## [Docker](https://www.docker.com/)

### [Official Docker 'Get Started' Documentation](https://docs.docker.com/get-started/)

#### Orientation

* `docker ps` List of your running containers.
* `docker --version` View version of Docker.
* `docker info` View more details of Docker installation.

#### Containers

* `docker run hello-world` Test if Docker is working correctly.
* `docker image ls` List out installed containers.
* `docker container ls` list running containers only.
* `docker container ls --all` list all containers.
* `docker build --tag=[tag-name] .` Build a Docker container named by the tag.
* `docker stop [container-name]` Stop container from running.
* `docker container stop [container-id]` Stop container from running.
* `docker run -p [4000]:[80] [container-name]` Run Docker container at published-port:exposed-port.
* `docker run -d -p [4000]:[80] friendlyhello` Run Docker container at published-port:exposed-port in detached mode (in the background).
* `docker login` Log in to Docker.
* `docker tag [container-name] [username]/[repository]:[tag]` Add a repository and tag to the registry.
* `docker push [username]/[repository]:[tag]` Upload tagged image to the registry.
* `docker run [username]/[repository]:[tag]` Run the image from a registry.

#### Services

* `docker swarm init` Initialize Docker swarm.
* `docker service ls` List current running services.
* `docker service ps [service-name]` List tasks for the service.
* `docker stack deploy -c docker-compose.yml [service-name]` Deploy your app based on docker-compose file configuration.
* `docker stack rm [service-name]` Stop the service/ app.
* `docker swarm leave --force` Take down the Swarm.

#### Swarms

* `docker swarm init` Enable Swarm mode and make the current machine a Swarm manager.
* `docker swarm join` Enable Swarm mode and join as a Worker.
* `docker-machine create --driver virtualbox [vm-name]` Create a VM locally using virtualbox.
* `docker-machine ls` List the VMs.
* `docker-machine ssh vm-name "docker swarm init --advertise-addr [vm-name-ip-address]"` Make the VM a swarm manager.
* `docker-machine ssh [vm-name] "docker swarm join --token [token] [swarm-manager-ip]:2377"` Add a worker to the swarm.
* `docker-machine ssh [vm-name] "docker node ls"` List the nodes in the swarm.
* `docker-machine ssh [vn-name] "docker swarm leave"` Remove that worker from the Swarm.
* `docker-machine ssh [vn-name] "docker swarm leave --force"` Remove the Swarm manager.
* `docker-machine env [vm-name]` Get the command to configure your shell to talk to the VM. Use the last line of the returned data to connect command shell to VM, such as `eval $(docker-machine env [vn-name])`.
* `eval $(docker-machine env -u)` Disconnect shell from VM.
* `docker stack rm [service-name]` Turn off the Swarm stack.
* `docker-machine stop [vm-name]` Stop VM node.
* `docker-machine start [vm-name]` Restart a VM node that has been stopped.

#### Deployment
* `docker node ls` List nodes in your Swarm.
* `docker service ls` List services.
* `docker service ps [service-name]` List tasks for a service.

### [Lynda - Learning Docker](https://www.lynda.com/Docker-tutorials/Learning-Docker/721901-2.html)

#### The Docker flow: Images to containers
* `docker images` List docker images.
* `docker run -ti [image-name]:[optional-tag] bash` The `-ti` flag is terminal interactive, it makes have a full terminal. Run an image in bash shell.
* `docker ps` View running images.

#### The Docker flow: Containers to images
* `docker ps -a` View all containers.
* `docker ps -l` View last exited container.
* `docker commit [container-id] [tag-name]` Commit container to image.

#### Run processes in containers

* `dock run --rm [container-name]` The `--rm` tag runs something in the container, and delete the container when it exits.
* `dock run [container-name] bash -c “sleep 3; echo all done”` Run the container, and executes things one after another (it sleeps for 3 seconds, then echos ‘all done’ to the terminal.
* `docker run -d -ti [container-name] bash` The `-d` tag runs the container detached, and leaves it running in the background.
* `docker attach [container-name]` Jump into running container. Use the keyboard sequence `ctl+p, ctl+q` to exit (detach) from the container and keep it running in the background.
* `docker exec [container-name]` Execute additional smaller commands (more for debugging) in a running container.

#### Manage containers

* `docker logs [container-name]` View logs for container.
* `docker kill [container-name]` Stops running container.
* `docker rm [container-name]` Deletes a container.
* `docker run --memory [maximum-allowed-memory] [image-name] [command]` Limit maximum memory for image.
* `docker run --cpu-shares [relative to other containers]` Limit CPU shares.
* `docker run —cpu-quota [limit in general]` Limit CPU to certain amount (e.g. this container can only run at a maximum of 10% of the CPU).

#### Network between containers

* `docker run -p [inside-port-number]:[outside-port-number]/[protocol (tcp is default/udp)]` The `-p` tag defines the arriving and departing ports.
* `docker port [container-name]` Display available container ports.

#### Link containers

* `docker run --rm --name [container-name]  [container-to-run]` The `--name` tag specifies the name of your container that will run.
* `nc -lp [port-number]` Inside a running container, use netcat `nc` to listen to port `-lp`.
* `docker run --rm -ti --link [container-to-link-to] --name [container-name] [container-to-run]` Run container and link to other container.

#### Dynamic and legacy linking

* `docker network create [network-name]` Create a network.
* `docker run --rm -ti --net=[network-name] --name [container-name] [container-to-run]` Create a container and connect to the network.
* `docker run -p 127.0.0.1:[inner-port-number]:[outer-port-number]/tcp` Have container listen to local connections only, creating a private service.

#### Images

* `docker commit [container-id] [tag-name]:[version-name]` Commit container to image with version-name. If version-name is omitted, it will default to ‘latest’.
* `docker push [organization]/[image-name]` Pushes image to the repository.
* `docker pull [organization]/[image-name]` Pulls image from the repository.
* `docker rmi [image-name / image-id]:[image-tag]` Removes image from your system.

#### Volumes

* `docker run -v [path/to/folder:/[shared-folder-on-container]] [container-name]` Creates a shared folder (volume) in a container.
* `docker run --volumes [container-name-with-volume] [container-to-run]` Create a container with shared volume.

#### Docker registries

* `docker search [search-term]` Search Docker registry.
* `docker login` Login to Docker account.
* `docker tag [container-name] [username]/[image-name]:[image tag]` Tag container image.
* `docker push [username]/[image-name]:[image-tag]` Push image to Docker registry.

#### What are Dockerfiles?

* `docker build -t [name-of-result] .` Create a Dockerfile tagged with `-t` tag.

#### Building Dockerfiles

* `FROM [image]/[tag]` What image the Dockerfile should be running from.
* `MAINTAINER [First-name] [Last-name] <[email]>` Defines who is the author of this file.
* `RUN [command-to-run]` Runs the command line.
* `ADD [whatever-to-add] [location-for-thing-to-be-added-to]` Adds whatever is needed to wherever you specify.
* `ENV [ENV_VAR]=[value]` Sets environment variable for the duration of the Dockerfile, and still be set in resulting image.
* `ENTRYPOINT` Specifies the start of the command to run.
* `CMD` Specifies the entire command to run (shell form: `nano notes.txt`, exec form: `["/bin/nano", "notes.txt"]`).
* `EXPOSE [port-number]` Maps port into container.
* `VOLUME [path-to-volume]` Defines shared or ephemeral volumes.
* `WORKDIR [path]` Sets the directory the container starts in (i.e. running `cd` in terminal).
* `USER [username/ number]` Sets the user the container will run as.

#### Multi-project Docker files

* `FROM [image]/[tag] as [command-name]` Name your command for later use.

#### Networking and namespaces

* `docker run --net=host --privileged=true [image-name]` The `--net=host` flag gives you unrestricted access to the host network. The `--privileged=true` flag gives it full control over the system hosting it.

#### Processes and cgroups

* `docker inspect [container-name]` Find out programs relating to the container.
* `docker run --pid=host [container-name]` The `--pid=host` turns off even more of the safeties.

#### Registries in detail

* `docker -d -p [incoming-port]:[outoging-port] --restart=always` The `--restart=always` flag tells the container to restart if it ever fails.
* `docker save -o [file-name] [image-name]:[tag]`
* `docker load -i [file-name]`
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
* ``
