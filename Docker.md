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

--- 
### DockerFile

A Dockerfile is a script that contains all commands for building a Docker image. The Dockerfile contains all instructions that will be used to create the Docker image with the 'docker build' command.
Some Dockerfile instructions

- `FROM` :Set the base-image for the new image that you want to create. The FROM instruction will initialize the new build-stage and must be located at the top of the Dockerfile.
- `LABEL` :With this instruction, you can add additional information about your Docker image, such as the version, description, maintainer, etc. The LABEL instruction is a key-value pair that allows you to add multiple labels and multi-line values.
- `RUN` :Instruction used to execute command during the build process of the docker image. You can install additional packages needed for your Docker images.
- `ADD` :ADD instruction is used to copy files, directories, or remote files from URL to your Docker images, from the `src` to the absolute path `dest`. Also, you can set up the default ownership of your file.
- `ENV` :The ENV instruction is used to define an environment variable that can be used during the build stage and can be replaced inline in many as well.
- `CMD` :The CMD instruction is used to define the default command to execute when running the container. And the Dockerfile must only contain one CMD instruction, and if there is multiple CMD, the last CMD instruction will be run.
- `EXPOSE` :This instruction is used to expose the container port on the specific network ports at runtime. The default protocol exposed is TCP, but you can specify whether the TCP or UDP.
- `ARG` :The ARG instruction is used to define a variable that the user can pass at the built-time. You can use this instruction in the docker `build command` during the build time using the `--build-arg variable=value` option and can be pass through the Dockerfile. Also, you can use multiple ARG at the Dockerfile.
- `ENTRYPOINT` :The ENTRYPOINT instruction is used to define the first and default command that will be executed when the container is running.
- `WORKDIR` :The WORKDIR instruction is used to define the default working directory of your Docker image. The RUN, CMD, ENTRYPOINT, and ADD instructions follow the WORKDIR instruction. You can add multiple WORKDIR instruction on your Dockerfile, and if there is doesn't exist, it will be created automatically.
- `USER` :The USER instruction is used to define the default user or gid when running the image. The RUN, CMD, and ENTRYPOINT follow the USER instruction in the Dockerfile.
- `VOLUME` :The VOLUME instruction ad used to enable access/linked directory between the container and the host machine.
