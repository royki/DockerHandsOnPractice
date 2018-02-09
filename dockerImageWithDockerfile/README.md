##### _To build docker image from Dockerfile_
- `docker build -t give_name_of_dockerImage path_file`
-  `docker build -t dockerImage1 .` 
  - * `.` if the command within the directory


##### _To Run Docker Image and Create a container_
- `docker run -p host_port:container_port --name give_name_of_container given_name_of_image`
- `docker run -p 81:80 --name dockerContainer1 dockerImage1`

##### _Run Docker Image with Volume between Host machine and Docker Container_
- `docker run -p 81:80 -v dir_path_of_host:dir_path_of_container --name give_name_of_container given_name_of_image`

- `docker run -p 81:80 -v /Users/rk/DockerTutorial/dockerImage1/src/:/var/www/html/ --name dockerContainer1 dockerImage1`
