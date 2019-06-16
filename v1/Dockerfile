# Maven Image
FROM maven:3.6.1-jdk-8-slim

# Update & Upgrade
RUN apt-get update && apt-get upgrade

# Set env
ENV JAVA_HOME /usr/local/openjdk-8

# /usr/share/maven
ENV MAVEN_HOME /usr/bin/mvn

CMD "mvn -v"

