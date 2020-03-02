# _Docker Command_

_Docker Run/Pull/Stop/Delete_
- `docker run --name "name_of_container" -d "container_image_name"`  
  - `-d` in daemon mode (run behind the process)
- `docker run --name "name_of_container" -d -t "container_image_name"` (to create the container and keep it up/running behind)
- `docker ps` (list of running docker containers/to identify a running container in docker)
- `docker ps -a` (list of all docker containers, that are not discarded yet)
- `docker inspect` (runtime information of container; listing metadata about a running or stopped container in json format)
- `docker inspect - '{{.NetworkSettings.IPAddress}}' CONTIANER_NAME/CONTAINER_ID` (retrieve only ip address of a running container)
- `docker inspect - '{{.NetworkSettings.Port}}' CONTIANER_NAME/CONTAINER_ID` (retrieve only port of a running container)
- `docker stop CONTIANER_NAME/CONTAINER_ID` (stopping a running container)
- `docker kill CONTIANER_NAME/CONTAINER_ID` (stopping a running container forcefully)
- `docker kill -s SIGKILL CONTIANER_NAME/CONTAINER_ID`
- `docker start CONTAINER_NAME/CONTAINER_ID` (to restart a stopped container)
- `docker start -ai CONTAINER_NAME/CONTAINER_ID` (to restart a stopped container and output of the command)
- `docker restart CONTIANER_NAME/CONTAINER_ID`
- `docker restart CONTIANER_NAME/CONTAINER_ID` (my-httpd-container)
- `docker rmi CONTIANER_NAME/CONTAINER_ID` (delete a container image from machine/cached)`
- `docker rm CONTIANER_NAME/CONTAINER_ID` (delete a container not the image)
- `docker rm $(docker ps -aq)` (deleting all container, `-q` returns only Id of containers)
- `docker stop $(docker ps -q)`
- `docker exec -it CONTAINER_NAME/CONTAINER_ID bash` (`exec`: to enter the running docker container; `it`: interactive terminal; to enter the running docker container/access the running container bash shell)
    - Example: `docker exec -it mysql bash`
- `docker logs CONTIANER_NAME/CONTAINER_ID` (to see the console log of the container)
    - Example: `docker logs mysql`

_Manipulating Container Images_
- To share image accross server, team , platform
    - Docker image registry(ies)
    - docker.io
    - registry.access.redhat.com

_Saving and Loading Images_
- `docker save`
    - `docker save [-o FILENAME] IMAGE_NAME[:TAG]` (`-o` is optional; generated image in standard output as binary data)
    - Example: `docker save -o mysql.tar.registry.redhat.com/rhsch/mysql-56-rhel17`
- `docker load`
    - `docker load [-i FILENAME]`
    - Example: `docker load -i mysql.tar`
    - (`docker gzip [-i FILENAME]`)
    - (`docker gunzip`) (before importing it to te daemon's caches history)

_Publishing Image to a Registry_
- `docker tag`
    - `docker tag IMAGE[:TAG][REGITRY_HOST/][USER_NAME/]NAME[:TAG]`
    - Example: `docker tag nginx nginx`
    - `docker tag mysql-custom devops/mysql` (to tag an image) (The mysql-custom option is the image name that is stored in the docker daemon's cache)
    - `docker tag mysql-custom devops/mysql:snapshots` (to use a different tag name)
    - **To associate multiple tags with a single image, use `docker tag` command**
    - Tags can be removed from the daemon using `rmi`
        - `docker rmi devops/mysql:snapshot`
    - **Because multiple tags can point to the same image, to removean image referred to by multiple tags, each tag should be individually removed first**

_Push an Image to the Registry_
- `docker push`
    - `docker push NAME[:TAG]`
    - Example: `docker push nginx`
- **Image name syntax**
    - `[registry_uri/][user_name/]image_name[:tag]` (tag normally version number)

**To delete from docker cache of any Image that is downloaded to docker cached**
- `docker rmi [OPTIONS] IMAGE [IMAGE..]` (Options: --force or -f to delete forcefully)
- `docker rmi $(docker images -q)` (deleting all images)

 _Modifying Images_
- `docker diff`
    - `docker diff CONTAINER`
    - Example: `docker diff mysql-basic`
- `docker commit`
    - `docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`
    - Example: `docker mysql-basic.mysql-custom`
    - Options available for `docker commit` 
        - `--author=" "`
        - `--message=" "`
    - `docker commit mysql-basic mysql-cutom` (to commit the changes to another image, run this command) !?

### _To Create a new image, there are two approaches_
- Using a running container
    - 
- Using a Dockerfile
    - `docker build -t DOCKER_IMAGE_NAME /path/DOCKER_FILE`
    - `docker run -p host_port:container_port --name container_name image_name`
- To mount volume within host and container
    - `docker run -p host_port:container_port --name container_name -v /path/folder:/path/in_container image_name`
    - Example: `docker run -p 81:80 --name localdocker -v /home/user/Docker/src:/var/www/html testdocker`
### _Docker Inspect_
- `docker inspect -f=='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name|ID`
- `docker inspect -f=='{{range .NetworkSettings.Networks}}{{.MacAddress}}{{end}}' container_name|ID`
- `docker inspect -f=='{{.LogPath}}' container_name|ID`
- `docker inspect --format='{{.Config.Image}}' container_name|ID`

### _Docker Log_
- `docker logs --details container_name|ID`
- `docker logs --tail all container_name|ID`
- `docker logs -f container_name|ID`

### _Run Docker in Remote_
- `docker -H=remote-docker-engine:PORT_NO`
- example - `docker -H=10.123.2.1:2375 run --name -d lb_nginx nginix:v1`

### _Run with resource limit_
- `docker run --cpus=.5 --name ubuntu_container -d ubuntu` [not more than 50% cpus]
- `docker run --memory=100m --name ubuntu_container -d ubuntu` [use 100 mb]

### _Docker Volume_
- `docker volume create data_volume` . It will create `data_volume` directory under `/var/lib/docker/volumes` folder. Then we can run to mount with container; `docker run -v data_volument:/var/lib/mysql --name -d mysql_db mysql`
- Volume Mount - mounts the directory in the volume folder & Bind Mount - mounts the directory in any location in the host machine.
- Using `-v` is old way. The new way is to use `--mount` option which is more verbose.
- Example: `docker run --mount type=bind, source=/data/mysql, target=/var/lib/mysql -d --name mysql_db mysql` 
- Docker common storage drivers - `AUFS`, `ZFS`, `BTRFS`, `Device Mapper`, `Overlay`, `Overlay2`
