FROM ubuntu:14.04

USER root

RUN apt-get update

# install cmake-3.x
RUN apt-get install -y software-properties-common
RUN add-apt-repository -y ppa:george-edison55/cmake-3.x
RUN apt-get update
RUN apt-get install -y make
RUN apt-get install -y cmake
RUN apt-get install -y curl
RUN apt-get install -y git

# install nodejs
RUN curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
RUN apt-get install -y nodejs

# install tmux
RUN apt-get update
RUN apt-get install -y python-software-properties software-properties-common
RUN add-apt-repository -y ppa:pi-rho/dev
RUN apt-get update
RUN apt-get install -y tmux=2.0-1~ppa1~t

ENV JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF8

WORKDIR /app
RUN git clone https://github.com/trilogy-group/lucida

WORKDIR /app/lucida/tools
RUN /bin/bash apt_deps.sh
RUN /bin/bash install_python.sh
RUN /bin/bash install_java.sh
RUN /bin/bash install_opencv.sh
RUN /bin/bash install_thrift.sh
RUN /bin/bash install_fbthrift.sh

# systemctl is not working in Docker
ADD tmp_install_mongodb.sh /app/lucida/tools
RUN /bin/bash tmp_install_mongodb.sh

WORKDIR /app/lucida

EXPOSE 3000
EXPOSE 27017

# Set default container command
ENTRYPOINT /usr/bin/mongod --config /etc/mongod.conf
