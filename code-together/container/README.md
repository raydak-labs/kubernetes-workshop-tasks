# Code together - Docker

## Part 1

```bash
# After setting docker up check its running state
docker info
# some details about what docker version ...

docker images
# displays downloaded images or built images

# okay lets run our first container
docker run --name=hello hello-world
# hello world should be printed. Great!

# Let's check our containers
docker ps
# nothing? hmmm... 
# The hello-world container stopped directly!
docker ps -a
# ah there it is

# lets keep everything clean
docker rm hello

docker ps -a
# good.

# lets run an nginx server (remove the container as soon as it stops with --rm)
docker run --rm nginx:alpine

# console will be attached to the container and you cannot execute other commands from here
# ctrl + c to stop the container. you could also detach with ctrl + z
docker run --rm -d --name=nginx nginx:alpine
# great runs in background. Lets check the logs
docker logs --tail=5 -f nginx
# ctrl+c

# lets jump into the container. Because its alpine -> ash 
docker exec -it nginx ash
# nice we are in the container. ctrl + d

docker stop nginx
docker rm nginx
# or quick: docker rm -f nginx

# Next step lets access the container from our host. For this we need to map the container port to the host.
# Lets map the internal port 80 -> to 8080 of the host
docker run --rm -p 8080:80 nginx:alpine

# Now try accessing http://localhost -> should display the nginx starting page
```

## Building images

Now we have used existing docker images.
Let's try building our own.
In the documentation of the nginx container we can find information on how to modify the displayed page.

```Dockerfile
FROM nginx:alpine

COPY ./sample.html /usr/share/nginx/html/index.html
```

Build the image `docker build -t myfirstimage:latest .`
That was easy.

Lets try running it: `docker run --rm -p 8080:80 myfirstimage:latest`

Works!

With `docker image ls` we can see our images.
For detailed information of the image we can run `docker image inspect myfirstimage:latest`

If we would want to release the image we could run `docker push myfirstimage:latest`
