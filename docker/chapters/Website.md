# Static Website Container

Lets create a static website container using **nginx**.

Create a directory **examples/nginx**

```sh
mkdir examples/nginx
cd examples/nginx
touch index.html
```
### index.html
```html
<html>
  <head>
    <title>Hello from Docker</title>
  </head>
  <body>
    <h1>Hello from Docker!!!</h1>
  </body>
</html>
```

Create the docker container
```sh
docker container run -d --name test-nginx -p 80:80 -v $(pwd):/usr/share/nginx/html:ro nginx:latest
```
* **-p** is a ports mapping <HOST PORT>:<CONTAINER PORT>.
* **-v** is a volume mounting <HOST DIRECTORY>:<CONTAINER DIRECTORY>.

Important: run command accepts only absolute paths. In our example we've used $(pwd) to set current directory absolute path.

Now you can check this [url](http://localhost/) in your browser.

We can try to change /example/nginx/index.html (which is mounted as a volume to directory /usr/share/nginx/html inside container) and refresh the page.

Let’s investigate some information about test-nginx container:
```sh
docker inspect test-nginx
```
This command displays system wide information regarding the Docker installation. Information displayed includes the kernel version, number of containers and images, exposed ports, mounted volumes, etc.

There are a lot of information in inspect output. If we just need to see the port mappings we can use port:
```sh
docker port test-nginx
```
### Console Output
```sh
80/tcp -> 0.0.0.0:80
```
