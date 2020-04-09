# _Containers, Kubernetes and Red Hat OpenShift_
- Docker
  - Docker architecture
  - Create Images
  - Dockerfile, Docker Compose
  - Tagging, Commit, Save, Load, Delete
- Kubernetes
- OpenShift 

## _Container(Docker) Architecture_
- Containers are segragated userspace environments for running applications.
- Images are templates of containers. Images are store in different registers. Registers can be privates,public, local on machine. (Public registry: Dockerhub)  
- _Namespaces_
  - Namespaces generally used isolated process to protect system resources and what Docker does it it creates a namespace for each individual container inside a Namespace,only process that are member of that namespace can see those resources.
    - Network Interface
    - Process Id list
    - Mount Points
    - IPC resources
    - System's on hostname information
- _Control Groups_
  - Control Groups creates partitions to set of processes and that is essentially protecting the host machine, from the containers consuming too many resources. In order to manage and limit the resources, it places restrictions on the amount of system resources; the processes belonging to a specific container might use.
- _SELinux_
  - SELinux is there to protect access between both the containers and the containers from the host. In adition `SVirt` uses `SELinux Multi Category Security(MCS)` to protect containers from each other.
- Docker Client (`docker pull`, `docker run`) - The command line tool is responsible for communicating with a server using RESTFul API to request operations.
- Docker Server/Host (docker daemon, local image, multiple container) - The service which runs as a daemon on an operating system, does heavy liftng of building, running and downloading container images. *The daemon can run either on the same system as the docker client or remotely.
  - *In RHEL, the daemon is represented by a `systemd` unit called `docker.service`
### _Docker Core Elements_
- Images: Images are read only templates that contain a runtime environment that includes application libraries and applications.
  - Images are used to create containes.
  - Images can be created, updated or downloaded for immediate consumptions.
- Registers: Registers store images for public or private use.
- Containers: Containers are segragated user-space environments for running applications isolated from other applications sharing the same host OS.
  - *In RHEL, the registry is represented by a `systemd` unit called `docker-registry.service`
