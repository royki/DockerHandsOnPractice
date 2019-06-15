FROM python:2.7.10-slim
# MAINTAINER royki

RUN printf "deb http://archive.debian.org/debian/ jessie main\ndeb-src http://archive.debian.org/debian/ jessie main\ndeb http://security.debian.org jessie/updates main\ndeb-src http://security.debian.org jessie/updates main" > /etc/apt/sources.list
RUN apt-get update && apt-get install -qq -y build-essential libpq-dev postgresql-client-9.4 --fix-missing --no-install-recommends

# create a path
ENV INSTALL_PATH /mobydock
# make directory
RUN mkdir -p ${INSTALL_PATH}
# set work directory
WORKDIR ${INSTALL_PATH}
# copy file to WORKDIRECTORY
COPY requirements.txt requirements.txt
# install requirements
RUN pip install -r requirements.txt

# `.` current directory
COPY . .
# set volume for static folder
VOLUME ["static"]
# run server 
CMD gunicorn -b 0.0.0.0:8001 "app.app:create_app()"