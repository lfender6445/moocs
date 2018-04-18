containers run images
images are downloaded from dockerhub

```sh
# start nginx

# downloads nginx
# starts it
# exposes local port 80 to container port 80
docker container run --publish 80:80 nginx
# to run detached
docker container run --publish 80:80 --detach nginx
```

docker maintains an image cache when it downloads it

gives us virtual ip on private network in docker engine
nginx start us using the command in the images docker file

containers arent vms
they are just processes
limited to what resources they can access
they exit when process stops

these process run on the host machine (nginx, mongo, etc)



```sh
docker image ls # list cached images
# start bash but when you exit it dies (takes nginx with it)
# this changes the nginx command to just use bash as the cmd
docker container run -it --name proxy nginx bash
# docker exec runs against already runnign process without interrupting the existing daemon
docker container exec -it mysql bash
```
