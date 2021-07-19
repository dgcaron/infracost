# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.185.0/containers/go/.devcontainer/base.Dockerfile
ARG VARIANT="1"
ARG TERRAFORM_VERSION="0.14.6"

FROM mcr.microsoft.com/vscode/devcontainers/go:0-${VARIANT}

RUN wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add - \
    && echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list \
    && apt-get update \
    && apt-get install -y mongodb-org=4.4.7 mongodb-org-server=4.4.7 mongodb-org-shell=4.4.7 mongodb-org-mongos=4.4.7 mongodb-org-tools=4.4.7 \
    && mkdir -p /var/lib/mongo && sudo chown `whoami` /var/lib/mongo \
    && mkdir -p /var/log/mongodb && sudo chown `whoami` /var/log/mongodb

RUN wget https://infracost-public-dumps.s3.amazonaws.com/cloudPricing.zip \
    && unzip cloudPricing.zip \
    && mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork \
    && mongorestore -d cloudPricing --gzip cloudPricing \
    && mongod --dbpath /var/lib/mongo --shutdown

RUN cd /tmp \
    && wget https://releases.hashicorp.com/terraform/0.14.6/terraform_0.14.6_linux_amd64.zip \
    && unzip terraform_0.14.6_linux_amd64.zip -d /usr/bin \
    && rm -rf /tmp/*