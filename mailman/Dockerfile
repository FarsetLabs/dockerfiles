#########################################
# Dockerfile to setup GNU Mailman Suite
# Based on Ubuntu
#########################################
# Set the base image to Ubuntu
FROM ubuntu

# File Author / Maintainer
MAINTAINER Terri Oda

# Update the sources and install some basic python stuff
RUN apt-get update && \
    apt-get install -y bzr python3-dev python3-pip python-dev python-pip python-virtualenv

# Get the nodejs stuff
RUN apt-get install -y nodejs npm && \
    npm install -g less && \
    ln -s /usr/bin/nodejs /usr/bin/node

# Set the default directory where CMD will execute
WORKDIR /mailman3

# Get Mailman Bundler
RUN bzr branch lp:mailman-bundler

# Get buildout, go into Mailman Bundler and build
RUN pip install zc.buildout
WORKDIR /mailman3/mailman-bundler
RUN buildout

# Set up virtualenv
RUN virtualenv venv
RUN . venv/bin/activate

# Expose ports
EXPOSE 8000

# Prep some stuff
RUN ./bin/mailman-post-update
RUN ./bin/mailman-web-django-admin createsuperuser
RUN ./bin/mailman start

# This sets the django webserver up to not only run, but binds it to all interfaces, port 8000
ENTRYPOINT ./bin/mailman-web-django-admin runserver 0.0.0.0:8000 
