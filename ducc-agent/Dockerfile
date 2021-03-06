# Dockerfile to set up Apache UIMA DUCC agent
# most of installation of UIMA DUCC deferred to run.sh script
#  see https://github.com/aleksey-hariton/uima-ducc-docker/blob/master/ducc-head/Dockerfile

FROM openjdk:8-jdk

# install needed software
RUN apt-get update && apt-get install -y make gcc net-tools openssh-server less dnsutils vim

# add local ducc user
RUN useradd -m ducc -s /bin/bash -d /home/ducc

# copy ssh key
# TODO this needs to be the same for head node and agents at the moment
COPY ./id_rsa* /home/ducc/.ssh/

# allow login using this key
RUN cp /home/ducc/.ssh/id_rsa.pub /home/ducc/.ssh/authorized_keys

# disable required user input at first connect
RUN echo "StrictHostKeyChecking=no" > /home/ducc/.ssh/config

# use the same key for root user
RUN cp -Rf /home/ducc/.ssh/ /root/
RUN chown -Rf root.root /root/.ssh/

# env vars needed by the agent:
# hostname of the DUCC head node
ENV TI_DOCKER_HEAD_HOST "textimager-server"
# memory limit
ENV TI_DOCKER_AGENT_MEMORY_LIMIT="1"
# id of docker group on host, get with:
# getent group docker | cut -d: -f3
ENV TI_DOCKER_HOST_GROUP_ID "99999"

# run script: finishes initialization and starts DUCC agent
COPY ./run.sh /home/ducc/

# set permission of copied files
RUN chown -Rf ducc.ducc /home/ducc/

CMD ["/bin/bash", "/home/ducc/run.sh"]
