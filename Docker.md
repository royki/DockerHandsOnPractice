- The benefits to using containers are numerous, particularly with Docker.  
- Docker images can run on many different platforms. If you can run an application on a Linux kernel, then you can package it in Docker, and then run it in different environments without having to change it.  
- Containers keep the concerns of development and operations separate. What happens inside the container doesn’t have much impact on operations that run outside the container. So, applications can do their own thing, while the process of running the container can be standardized.  
- Containers are easy to manage – you can quickly create and delete, download, share, start and stop them. 
- Containers use hardware resources more efficiently than virtual machines, and are more lightweight. They are more dense than virtual machines. Docker containers can share layers, and support multi-tenancy.  
- Containers should be treated as unchangeable, so that every container created from a single image will be the same. Container images are versions. You should not patch a container, but patch the image when it needs to be updated. When you don’t need it anymore, a container can be easily deleted or replaced.

- In Docker, a union file system is the union of - `read-write layer and all read-only layers`
- Docker Images don't have different states and keep changing with time.
- In what format are Docker images identified? - `64 hexadecimal digit string`
- In Docker, when a container is exited - `the state of the filesystem is stored but the processes are not.
- What is the command used to convert a container into an image? - `docker commit`
- The default registry is accessible using the command: `sudo docker search`
- Which of the following command is used to associate an image with a repository (or multiple repositories) at build time? - `sudo docker build -t IMAGENAME`
- `docker ps` shows all running containers by default.
- By default, in what mode do Docker containers run in? And what is the value of the -d option that specifies the mode in which docker container runs? `foreground mode; -d =True`
- docker diff command has 3 events listed in it: `A- Add; D-Delete; C-Change;`
