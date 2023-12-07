# Use a base image of python cd 
FROM python:latest

# Install build dependencies
RUN apt-get update && \
    apt-get install -y build-essential wget

RUN apt-get install -y libssl-dev zlib1g-dev libncurses5-dev libncursesw5-dev libreadline-dev libsqlite3-dev libgdbm-dev libdb5.3-dev libbz2-dev libexpat1-dev liblzma-dev libffi-dev libc6-dev



RUN apt install -y gawk
RUN apt install -y wget
RUN apt install -y git
RUN apt install -y diffstat
RUN apt install -y unzip
RUN apt install -y texinfo
RUN apt install -y gcc
RUN apt install -y build-essential
RUN apt install -y chrpath
RUN apt install -y socat
RUN apt install -y cpio
RUN apt install -y python3
RUN apt install -y python3-pip
RUN apt install -y python3-pexpect
RUN apt install -y xz-utils
RUN apt install -y debianutils
RUN apt install -y iputils-ping
RUN apt install -y python3-git
RUN apt install -y python3-jinja2
RUN apt install -y libegl1-mesa
# RUN apt update && apt install -y pylint3
RUN apt install -y xterm
RUN apt install -y python3-subunit
RUN apt install -y mesa-common-dev
RUN apt install -y zstd
RUN apt install -y liblz4-tool

# Install text editor
RUN apt install nano

# Install locales
RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

# Set environment variables, if needed
ENV HOME /home/myuser

# Create a non-root user
RUN useradd -ms /bin/bash myuser

# Set the user as the default user for subsequent commands
USER myuser

# Set the working directory
WORKDIR /home/myuser

# # Clone yocto
# RUN mkdir yocto_tutorial
# RUN cd yocto_tutorial
# RUN git clone git://git.yoctoproject.org/poky -b kirkstone
# RUNd poky
# source oe-init-build-env



# Define el comando predeterminado cuando se inicia el contenedor
CMD ["tail", "-f", "/dev/null"]