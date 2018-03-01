# Docker Container using Dockerfile
Lets build a docker container using Dockerfile. Dockerfile is a text file that contains list of instructions.

## Dockerfile
To build a Docker image you need to write Dockerfile. It’s a plain text file with instructions and its arguments. Here is the description of instructions we’ll use in next example:

* **FROM** - set base image
* **RUN** - execute command in container
* **ENV** - set environment variable
* **WORKDIR** - set working directory
* **VOLUME** - create mount-point for volume
* **CMD** - set executable for container

You can find the full list of instructions [here](https://docs.docker.com/engine/reference/builder/)

Let’s create an image that will get site contents with curl and store it to the text file. We’ll pass site url via environment variable SITE_URL. Resulting file will be placed in a directory mounted as volume. Here is our Dockerfile:

```sh
FROM ubuntu:latest
RUN apt-get update && apt-get install -y curl wget
ENV SITE_URL https://www.google.com/
WORKDIR /data
VOLUME /data
CMD sh -c "curl -L $SITE_URL > /data/results"
```

```sh
docker build . -t test-curl
```

* docker build is a command to build a new image locally.
* -t set the name:tag to image.

Now we have our new image and we can see it in list of existing images:
```sh
docker images
```
### Console Output
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test-curl           latest              2ceb91527cbc        48 seconds ago      162MB
nginx               latest              e548f1a579cf        8 days ago          109MB
ubuntu              latest              0458a4468cbc        4 weeks ago         112MB
hello-world         latest              f2a91732366c        3 months ago        1.85kB
sandbox-hdp         latest              c3cef4760133        7 months ago        12.2GB
```

So we can create and run container from image. Let’s try with default parameters:
```sh
docker run --rm -v $(pwd)/vol:/data/:rw test-curl
```
### Console output
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 11587    0 11587    0     0  74467      0 --:--:-- --:--:-- --:--:-- 74754
```
To see results saved to file run cat ./vol/results.

Let’s try with amazon.com:
```sh
docker run --rm -e SITE_URL=https://amazon.com/ -v $(pwd)/vol:/data/:rw test-curl
```
### Console output
```sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   179  100   179    0     0    142      0  0:00:01  0:00:01 --:--:--   142
100  2671  100  2671    0     0   1508      0  0:00:01  0:00:01 --:--:-- 1304k
```
Again to see results run cat ./vol/results.
