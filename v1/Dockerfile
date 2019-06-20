# Maven Image with Java8
FROM maven:3.6.1-jdk-8-slim AS base

# Set MAVEN_HOME
# /usr/share/maven
ENV MAVEN_HOME /usr/bin/mvn

# COPY pom.xml file to Docker Image
COPY pom.xml .

# Download all dependencies
# RUN mvn dependency:go-offline --fail-never
# RUN mvn verify clean --fail-never
RUN mvn install -Dmaven.test.skip=true

FROM openjdk:8u171-jre-alpine as maven

# Install maven
RUN apk add maven && apk update

# /usr/bin/java
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk/jre
ENV MAVEN_HOME /usr/bin/mvn

# Copy from base Image
COPY --from=base /root/.m2 /root/.m2

# Set Work Dir env
ENV PROJECT_PATH /project
# Create Work Dir 
RUN mkdir -p ${PROJECT_PATH}
# Set Work Dir
WORKDIR ${PROJECT_PATH}

# COPY to Image
COPY . .

# Mount Source Dir (If needed)
# VOLUME ["src"]

# Set the default fleetId. During build time of the image
ARG default_envId "PUT ID"

# Set envId as env variable to pass the value during container runtime
ENV envId = ${default_envId}

# `stationId` is the maven project `propertyValue`
CMD "mvn clean test -DenvId=${envId} site"

