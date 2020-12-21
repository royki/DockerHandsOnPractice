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
- `docker restart CONTIANER_NAME/CONTAINER_ID`  (my-httpd-container)
- `docker rmi IMAGE_NAME/IMAGE_ID` (delete a container image from machine/cached)`
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
    - **Because multiple tags can point to the same image, to remove an image referred to by multiple tags, each tag should be individually removed first**

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
     - `docker run -p host_port:container_port --name container_name image_name`
- Using a Dockerfile
    - `docker build -t DOCKER_IMAGE_NAME /path/DOCKER_FILE`   
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
- Image Layers - `Read Only`; Container Layer - `Read Write`
- COPY-ON-WRITE - Write/Update of a file can only apply on Container Layer or Read Write Layer.
- `docker volume create data_volume` . It will create `data_volume` directory under `/var/lib/docker/volumes` folder. Then we can run to mount with container;
   - `docker run -v data_volument:/var/lib/mysql --name -d mysql_db mysql`
- Two Types of Volume
  - Volume Mount - mounts a volumn creating by docker & mounts the directory in the `/var/lib/docker/volume` directory.
  - Bind Mount - mounts the directory in any location in the host machine.
- Using `-v` is old way. The new way is to use `--mount` option which is more verbose.
- Example: `docker run --mount type=bind, source=/data/mysql, target=/var/lib/mysql -d --name mysql_db mysql` 
- Storage Drivers
  - Docker common storage drivers - `AUFS`, `ZFS`, `BTRFS`, `Device Mapper`, `Overlay`, `Overlay2`
- Volume Drivers (Volumes are handled by Volume Driver Plugins)
  - The default Vuloumn Driver Plugin is `Local`. The Local Volume Driver Plugin helps creating a volumn in Docker Host Machine and store the data under `/var/lib/docker/volumes` directory.
  - Volume Driver Provider: `Local`, `Azure File Storage`, `Convoy`, `DigitalOcean Block Storage`, `Flocker`, `GCE-Docker`, `GlusterFS`, `NetApp`, `PortWox`, `RexRay` etc.
  - - Example: `docker run -it --name mysql-con --volume-driver rexray/ebs --mount src=ebs-vol, target=/var/lib/mysql mysql`
  
### Container Storage Interface (CSI)

- Container Runtime - `rkt`, `cri-o`. 
- Container Runtime Interface(CRI) - `rkt`, `cri-o`
- Container Network Interface(CNI) -  `weaveworks`, `flannel`, `cilium`
- Container Storage Interface(CSI) - `portwox`, `Amazon EBS`, `GlusterFS`
- CSI is not built on K8S specific standard rather built on general storage standard that can support any storage vendor.
- Kubernetes, CloudFoundry & Mesos are on board with CSI.
- Set of RPC 
  - CreateVolume
  - DeleteVolume
  - ControllerPublishVolume

### _Docker Network_
- `Bridge`, `None`, `Host`
- `docker run ubuntu --network=none` or `docker run ubuntu --network=host`. Default is `bridge`
- Bridge network range start from `172.17.0.0`
- By default docker create one internal network. We can create as well internal network as - `docker network create --driver=bridge --subnet=182.18.0.0/16 custom-isolated-network`
- `docker network ls`
- `docker inspect container_id`
- docker container DNS always run at `127.0.0.11` port number.
- `docker network create wp-mysql-network --driver=bridge --subnet=182.18.0.1/24 --gateway=182.18.0.1`
- `docker network ls`
- `docker inspect alpine-1 | grep -i network`
- `docker network inspect bridge`
- `docker run -d --name alpine-2 --network none alpine`
- `docker network create wp-mysql-network --driver=bridge --subnet=182.18.0.1/24 --gateway=182.18.0.1`
- `docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 --network wp-mysql-network mysql`
- `docker network inspect wp-mysql-network`
- `docker run -d --name webapp -e DB_Host=mysql-db --network wp-mysql-network kodekloud/simple-webapp-mysql`
- `docker network inspect wp-mysql-network`

### _Docker Registry_
- Docker public DNS is Docker Hub -> `docker.io`
- image: `Registry/User_Account/Image_Or_Repository` -> `docker.io/ngnix/nginx`
- Run from private registry: `docker run private-registry.io/apps/internal-app`
- Docker registry is itself an application and availabe as an image and can be deployed as container in local.
- Deploy docker registry in locally - `docker run -d --name local-registry -p 5000:5000 registry:2`
- `docker image tag my-image localhost:5000/my-image`
- Push image in local registry - `docker push localhost:5000/my-image`
- Pull image from local registry - `docker pull localhost:5000/my-image`
- Pull image from another host registry - `docker pull 192.168.56.100:5000/my-image`

### _Limiting Memory & CPU Usage_

Follow the command `docker info`
To Change this - `WARNING: No swap limit support`
*to Configure/Enable* from `vi /etc/default/grub`
  - ` GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=0"` from `GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"`
  - `sudo update-grub`

- `docker stats` or `docker stats container_name` or `docker stats container_id`
- Limiting Memory Usage
  - Hard Limit 
    - `docker run --name lb_nginx -d -p 8081:80 --memory="256m" nginx`
  - Soft Limit 
    - `docker run --name lb_nginx -d -p 8081:80 --memory-reservation="256m" nginx` 
    - `docker run --name lb_nginx -d -p 8081:80 --memory="256m"--memory-reservation="512m" nginx` 
  - Using Memory Swap (To limit a container's use of memory swap to disk use)
    - `docker run --name lb_nginx -d -p 8081:80 --memory="256m" --memory-swap="512m" nginx`
- Limiting CPU Usage
  - `docker run --name lb_nginx -d -p 8081:80 --cpus=".5" nginx`
  - To limit a container’s CPU shares use `--cpus-shares`
    - `docker run --name lb_nginx -d -p 8081:80 --cpus-shares="512" nginx`

### _Security_

- `/usr/include/linux/capabilites.h`
- `docker run --cap-add NET_BIND ubuntu`
- `docker run --cap-drop KILL ubuntu`
- `docker run --privileged ubuntu`

---
Create a docker-compose.yml file under the path /root/wordpress. Once done, run a docker-compose up.
```yaml
version: '3.0'
services:
  db:
    environment:
      POSTGRES_PASSWORD: mysecretpassword
    image: postgres
  wordpress:
    image: wordpress
    links:
    - db
    ports:
    - 8085:80
```
- `docker-compose -f docker-compose.yaml config`
- `docker-compose up`
---
- Create a container with volume
- `docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -v /opt/data:/var/lib/mysql -d mysql`
- To view the data in mysql-db container
- `docker exec mysql-db mysql -pdb_pass123 -e 'use foo; select * from myTable'`
---
- `docker network ls`
- What is the subnet configured on bridge network?
- `docker inspect bridge`
- Run a container named alpine-2 using the alpine image and attach it to the none network.
- `docker run --name alpine-2 --network=none -d alpine`
- Create a new network named wp-mysql-network using the bridge driver. Allocate subnet 182.18.0.1/24. Configure Gateway 182.18.0.1
- `docker network create wp-mysql-network --subnet=182.18.0.1/24 --gateway=182.18.0.1`
- Deploy a mysql database using the mysql:5.6 image and name it mysql-db. Attach it to the newly created network wp-mysql-network. Set the database password to use db_pass123. The environment variable to set is MYSQL_ROOT_PASSWORD
- `docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 --network=wp-mysql-network -d mysql:5.6`
- Deploy a web application named webapp, using image kodekloud/simple-webapp-mysql. Expose port to 38080 on the host. The application takes an environment variable DB_Host that has the hostname of the mysql database. Make sure to attach it to the newly created network wp-mysql-network
- `docker run --network=wp-mysql-network -e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 --name webapp --link mysql-db:mysql-db -d kodekloud/simple-webapp-mysql`
