FROM xfrarod/chef-node:v1 as chefnode

FROM xfrarod/chef-server:v1 as chefserver

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

RUN adduser --home /home/jenkins --gid 0 jenkins <<  EOF\
jenkins\
jenkins\
jenkins\
00\
99\
00\
99\
Y\
EOF

RUN mkdir /workspace && chown -R jenkins:root /workspace

RUN apt-get update && \
    apt-get install -y default-jre git python python-pip python-setuptools python-dev \
    ca-certificates build-essential openssh-server openssl software-properties-common && \
    wget -q https://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/${SWARM_CLIENT_VERSION}/swarm-client-${SWARM_CLIENT_VERSION}.jar -P /home/jenkins/
#&& \
#    pip install docker-compose

RUN add-apt-repository ppa:cpick/hub && \
    apt-get update && \
    apt-get install -y hub sudo

RUN usermod -aG sudo jenkins
RUN echo 'jenkins   ALL=(ALL:ALL) NOPASSWD:ALL' > /etc/sudoers.d/jenkins

# Install docker

RUN apt install -y apt-transport-https ca-certificates curl software-properties-common && \
    echo 'deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable' > /etc/apt/sources.list.d/docker.list &&  \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
    apt update && \
    apt install -y docker-ce && \
    docker --version

COPY run.sh /run.sh
RUN chmod +x /run.sh

ENTRYPOINT ["/run.sh"]

# Install chef-server configuration

COPY --from=chefnode /root/.chef/frrodriguez.pem /home/jenkins/.chef/frrodriguez.pem
COPY --from=chefnode /root/.chef/credentials /home/jenkins/.chef/credentials

RUN cd /home/jenkins/.chef && sed -i "s|root|home/jenkins|g" credentials

COPY --from=chefserver /var/opt/opscode/nginx/ca/chef-server.crt /home/jenkins/.chef/trusted_certs/chef-server.crt

RUN chown -R jenkins:0 /home/jenkins/.chef

#RUN knife configure << EOS\
#https://chef-server/organizations/donus\
#frrodriguez\
#Y\
#EOS

USER jenkins:0
RUN gem install kitchen-docker
RUN chef gem install kitchen-docker
RUN gem install rubocop
RUN gem install rubocop-checkstyle_formatter
RUN echo "cookbook_path   = './cookbook'" >> ~/.chef/credentials

USER root
RUN gem install rubocop-checkstyle_formatter

