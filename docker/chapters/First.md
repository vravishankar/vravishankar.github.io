# Docker First Container

### Command

Docker commands are in the format:

docker < master-command > < sub-command > < command line>

```sh
docker container run ubuntu /bin/echo 'Hello World'

```

* **docker container run** is a command to run a container.
* **ubuntu** is the image you run, for example, the Ubuntu operating system image. When you specify an image, Docker looks first for the image on your Docker host. If the image does not exist locally, then the image is pulled from the public image registry — Docker Hub.
* **/bin/echo ‘Hello world’** is the command that will run inside a new container. This container simply prints Hello world and stops the execution.

### Console Output

```
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
1be7f2b886e8: Pull complete 
6fbc4a21b806: Pull complete 
c71a6f8e1378: Pull complete 
4be3072e5a37: Pull complete 
06c6d2f59700: Pull complete 
Digest: sha256:e27e9d7f7f28d67aa9e2d7540bdc2b33254b452ee8e60f388875e5b7d9b2b696
Status: Downloaded newer image for ubuntu:latest
HelloWorld
```

## Container using interactive shell

```sh
docker container run -it --rm ubuntu /bin/bash
```

* -t flag assigns a pseudo-tty or terminal inside the new container.
* -i flag allows you to make an interactive connection by grabbing the standard input (STDIN) of the container.
* -— rm flag to automatically remove the container when the process exits. 

By default, containers are not deleted. This container exists until we keep the shell session and terminates when we exit from the session (like SSH session to a remote server).

If you want to keep container running after the end of the session, you need to daemonize it:

```sh
docker run --name hello-os -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

* **— name hello-os** assigns daemon name to a new container. If you don’t specify a name explicitly, Docker will generate and assign it automatically.
* -d flag runs the container in the background (daemonize it).

Let’s see what containers we have at the moment:

```sh
docker ps -a
```

### Console Output
```
CONTAINER ID        IMAGE               COMMAND                  CREATED                  STATUS                      PORTS               NAMES
f06245dccd23        ubuntu              "/bin/sh -c 'while..."   Less than a second ago   Up 15 seconds                                   hello-os
c2eeb894d5f6        ubuntu              "/bin/echo HelloWorld"   25 minutes ago           Exited (0) 25 minutes ago                       trusting_mcclintock
e20be3634ef6        hello-world         "/hello"                 2 days ago               Exited (0) 2 days ago
```

* **docker ps** is a command to list containers.
* **-a** shows all containers (without -a flag ps will show only running containers).
The ps shows us that we have two containers:

**trusting_mcclintock** (name for this container was generated automatically — it will be different on your machine) — it’s the first container we’ve created which printed ‘Hello world’ once.
**hello-os** — it’s the third container we’ve created which runs as a daemon.
Note: there is no second container (the one with interactive shell) because we set — rm option. As a result, this container is automatically deleted right after execution.

Let’s check the logs and see what daemon container is doing right now:

```sh
docker logs hello-os
```

### Console Output
```sh
...
hello world  
hello world  
hello world
```

* **docker container logs** fetches the logs of a container.
* -f flag to follow the log output (works actually like tail -f).
Now let’s stop daemon container:

```sh
docker container stop hello-os
```

Lets check if the container is stopped
```sh
docker container ps -a
```

### Console Output
```sh
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
f06245dccd23        ubuntu              "/bin/sh -c 'while..."   22 minutes ago      Exited (137) 4 seconds ago  
```

The container is stopped. We can start it again:

```sh
docker container start hello-os
```
Let’s ensure that it is running:
```sh
docker container ps -a
```
### Console output
```sh
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
f06245dccd23        ubuntu              "/bin/sh -c 'while..."   25 minutes ago      Up 7 seconds                                       hello-os
c2eeb894d5f6        ubuntu              "/bin/echo HelloWorld"   About an hour ago   Exited (0) About an hour ago                       trusting_mcclintock
```

Now let’s stop it again and remove all the containers manually:

```sh
docker stop hello-os  
docker rm trusting_mcclintock 
docker rm hello-os
```
To remove all containers we can use the following command:
```sh
docker container rm -f $(docker ps -aq)
```
* **docker container rm** is command to remove container.
* **-f** flag (for rm) is to stop container if it’s running (force deletion).
* **-q** flag (for ps) is to print only container IDs.
