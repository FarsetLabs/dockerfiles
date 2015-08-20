FROM ubuntu:14.04
MAINTAINER Dan Porter

# install dependencies from apt
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y bzr curl node-less python3-dev python3-pip python-dev python-pip python-virtualenv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# install Node stable
RUN curl -sL https://deb.nodesource.com/setup | bash - && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

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
