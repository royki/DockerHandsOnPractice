# Maven Image with Java8
FROM maven:3.6.1-jdk-8-slim

# Update & Upgrade
RUN apt-get update && apt-get upgrade

# Set JAVA_HOME env
ENV JAVA_HOME /usr/local/openjdk-8

# Set MAVEN_HOME
# /usr/share/maven
ENV MAVEN_HOME /usr/bin/mvn

# Set Work Dir env
ENV PROJECT_PATH /project_api
# Create Work Dir 
RUN mkdir -p ${PROJECT_PATH}
# Set Work Dir
WORKDIR ${PROJECT_PATH}

# COPY pom.xml file to Docker Image
COPY . .

# Mount Project Dir to Docker Image
VOLUME ["src"]

ARG stationId "PUT ID"

# `stationId` is the maven project `propertyValue`
CMD "mvn clean test -DstationId=${stationId} site"

