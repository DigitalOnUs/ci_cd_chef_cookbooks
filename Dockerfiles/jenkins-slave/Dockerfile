FROM chef/chefdk:3.5.13

MAINTAINER Francisco Rodriguez <francisco.rodriguez@digitalonus.com>
ARG DOCKER_VERSION="17.12.1"

ARG version="0.2.0"
ARG build_date="unknown"
ARG commit_hash="unknown"
ARG vcs_url="unknown"
ARG vcs_branch="unknown"

LABEL org.label-schema.vendor="vfarcic" \
    org.label-schema.name="jenkins-swarm-agent" \
    org.label-schema.description="Jenkins agent based on the Swarm plugin" \
    org.label-schema.usage="/src/README.md" \
    org.label-schema.url="https://github.com/vfarcic/docker-jenkins-slave-dind/blob/master/README.md" \
    org.label-schema.vcs-url=$vcs_url \
    org.label-schema.vcs-branch=$vcs_branch \
    org.label-schema.vcs-ref=$commit_hash \
    org.label-schema.version=$version \
    org.label-schema.schema-version="1.0" \
    org.label-schema.build-date=$build_date

ENV SWARM_CLIENT_VERSION="3.9" \
    DOCKER_COMPOSE_VERSION="1.19.0" \
    COMMAND_OPTIONS="" \
    USER_NAME_SECRET="" \
    PASSWORD_SECRET=""

RUN apt-get update && \
    apt-get install -y default-jre git python python-pip python-setuptools python-dev \
    ca-certificates build-essential openssh-server openssl software-properties-common && \
    add-apt-repository ppa:cpick/hub && \
    apt-get update && \
    apt-get install -y hub sudo
    
# Install docker
RUN apt install -y apt-transport-https ca-certificates curl software-properties-common && \
    echo 'deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable' > /etc/apt/sources.list.d/docker.list &&  \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
    apt update && \
    apt install -y docker-ce && \
    docker --version

RUN useradd -ms /bin/bash -d /home/distro -u 1033 distro && \
    groupdel docker && \
    groupadd docker -g 1045 && \
    usermod -a -G docker distro

  
RUN chef gem install kitchen-docker
RUN gem install kitchen-docker
RUN gem install rubocop
RUN gem install rubocop-checkstyle_formatter
RUN gem install json github

USER distro   
RUN chef gem install kitchen-docker && gem install kitchen-docker
