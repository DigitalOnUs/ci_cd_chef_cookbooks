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


RUN echo "knife[:aws_access_key_id] = ENV['AWS_ACCESS_KEY_ID']" > ~/knife.rb && \
    echo "knife[:aws_secret_access_key] = ENV['AWS_SECRET_ACCESS_KEY']" >> ~/knife.rb && \
    echo "" >> ~/knife.rb && \
    echo "current_dir = File.dirname(__FILE__)" >> ~/knife.rb && \
    echo "log_level              :info" >> ~/knife.rb && \
    echo "log_location           STDOUT" >> ~/knife.rb && \
    echo "node_name              'ubuntu'" >> ~/knife.rb && \
    echo "client_key             '/etc/chef/client.pem'" >> ~/knife.rb && \
    echo "validation_client_name 'tlinfra-chef12-ci.thislife.com'" >> ~/knife.rb && \
    echo "validation_key         '/home/ubuntu/chef12/.chef/thislife-chef12-validator.pem'" >> ~/knife.rb && \
    echo "chef_server_url        'https://tlinfra-chef12-ci.thislife.com:443/organizations/thislife'" >> ~/knife.rb && \
    echo "trusted_certs_dir      '/home/ubuntu/chef12/.chef/trusted_certs'" >> ~/knife.rb && \
    echo "cookbook_path          '../cookbooks'


RUN echo '-----BEGIN RSA PRIVATE KEY-----' > /etc/chef/client.pem && \
    echo 'MIIEogIBAAKCAQEAznXE9JBpsrmtWFSM3YBU9WFbnsR3PTw05IfSAQfvsl4yxeIE' >> /etc/chef/client.pem && \
    echo 'U11IeN8f3awWCqGWPqNsuqFUfLpCf9fAQ0KnGi2b5B7eNX0uvxWimNjL9JSPQCeE' >> /etc/chef/client.pem && \
    echo 'cIWi+CkubzDQ3LB3r4C5GudHswqh7ToHnVm8uuBdMN1wSVAi7FF/krNcuZQyaoh2' >> /etc/chef/client.pem && \
    echo 'tTVK6Pf3P9NrYrbrXIY5gZdM11MRnual4I69RjPfcsut0WSNYK8NugCenU2omfi8' >> /etc/chef/client.pem && \
    echo 'Gszs1fmR36Avp4c50uLsN2wszXiBLoJD2Qo0UEGfHNwG/ZjjaBZf4q+4XZcXYq4F' >> /etc/chef/client.pem && \
    echo 'KwWGKY4fayuwQE7z2dAgO52GYjNB17iAsoxXtwIDAQABAoIBAHPXmJ8wuF3T2POK' >> /etc/chef/client.pem && \
    echo '3U8bRpA4BFRdIBp3waXIn4h8j32hZROq8Ypk+Nj5OMQEXomwiMC0qbk99E3XpKBe' >> /etc/chef/client.pem && \
    echo 'yj3zFBeYJ7QjrcKNbsFAvm7MlgOxp37zrVgFY5+iW69/0HFDgqQysanqw0wuVxTo' >> /etc/chef/client.pem && \
    echo 'ToV3aHHimedlGC/Kiw3V3KLPiOOGxZQhsIw7gokVG/kyevHQyWRQ/RraTmaQgZ6a' >> /etc/chef/client.pem && \
    echo 'KrCz1XjU2xwwaS49fQZXy12uGwochO++rEsYQvM2E7Wg4DiuqxbRapsjm1AMB1Gx' >> /etc/chef/client.pem && \
    echo 'UhO+U5rK0R7vTjNUz0YENz7mManNqxHc7djmfO8zDVtBuKs/ijRwf8mpL9Yjdhkv' >> /etc/chef/client.pem && \
    echo 'B+D+CGECgYEA5zoDM0yYDDv/oQJOeqUhFcLJrwMwQlwyVC8OM7q2sVOmkv47t47R' >> /etc/chef/client.pem && \
    echo 'UV0NpWVbaUk+FeYYvDfWXhQ4XePBgco8N/FVkgLybXAuYlevmQpWcyKtuBGvPLfz' >> /etc/chef/client.pem && \
    echo 'v0P69BqKuuLE1nMy67tApqVc9OEFAAbf1R58dSPVN8uPgM/NAyRMg5MCgYEA5JR4' >> /etc/chef/client.pem && \
    echo 'Mr4a31mubEBe2CY8uoZ6OgtJNTFP97o6IAi52FeCjM2X6q9bGXT2lJj46YbNODmZ' >> /etc/chef/client.pem && \
    echo 'r1cG1YU8RIb2gOh3rQxEMApTxk0A4vSdc+KcM0nTbDmX6SLmCPhj9FK6+L7pnIdG' >> /etc/chef/client.pem && \
    echo 'v53MWevW9wiyEINzXt052jIJIAEwL6nmTa+f+c0CgYB3NrzBrSLTY/e3DpZhu1Nk' >> /etc/chef/client.pem && \
    echo 'gco0FmPCQwCb0Kr01rYslOFT9gnZ6KNY3K4VcT2A+gctiTWFjRBT7v7kwXFaNmd3' >> /etc/chef/client.pem && \
    echo 'vFCO+D//VpBDmcvqDp9luyJvvYZourrMCQOQ6UJPem1b14cQbLm3cZiMlJtcWS78' >> /etc/chef/client.pem && \
    echo 'FTy7tF7Q5n21Pu4HSaa9SwKBgDS8TD7ovSpqdmNjNn6H09r0y544nFs5VUnCHUbm' >> /etc/chef/client.pem && \
    echo 'vHm9NtFlXXc3nbzxzzHZWRQHlbEzGNHrMhhw9MaanKSA/ZtfCRxMRaeqfFngNbNW' >> /etc/chef/client.pem && \
    echo 'xNLQZYbCgtm2w3uuUuE7dQeOhDcovpnWvh7A2oeD6hEFQsjQIVjF0nUf0Q4uFIQ3' >> /etc/chef/client.pem && \
    echo '0KDBAoGANt44XfRqnN+zdLQFvXSqAA2GiVXvcMMEtLD1B+cfnuLcIwtiYXhug4fH' >> /etc/chef/client.pem && \
    echo 'YnJUPr5JX8Opf8bsAPIf31GF2gBPpAdL6UIhD9xnk1/X6LsOoR/tBK6ibSGkKpaa' >> /etc/chef/client.pem && \
    echo 'hqdQgYrB/lYEFkYPLPu6Dz/qnRd3btaEExsHDXgD+x9KWoJmRCw=' >> /etc/chef/client.pem && \
    echo '-----END RSA PRIVATE KEY-----' >> /etc/chef/client.pem

USER distro   
RUN chef gem install kitchen-docker && gem install kitchen-docker
