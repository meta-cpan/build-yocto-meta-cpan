FROM ubuntu:18.04

ENV DEBIAN_FRONTENV noninteractive

RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y apt-utils locales

RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
        echo 'LANG="en_US.UTF-8"'>/etc/default/locale && \
        dpkg-reconfigure --frontend=noninteractive locales && \
        update-locale LANG=en_US.UTF-8

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Required Packages for the Host Development System
# https://www.yoctoproject.org/docs/latest/mega-manual/mega-manual.html#brief-build-system-packages
RUN apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping libsdl1.2-dev xterm

# Required packages for meta-cpan, meta-jens and meta-gpw
RUN apt-get install -y docbook2x perlbrew libstatgrab-dev libssl-dev

# Create a non-root user that will perform the actual build
RUN id poed 2>/dev/null || groupadd --gid 666 iot
RUN id poed 2>/dev/null || useradd --uid 500 -g staff -G iot --create-home poed

# Create CI user
RUN id ci 2>/dev/null || useradd --uid 501 -g staff -G iot --create-home ci

RUN apt-get install -y sudo
RUN echo "poed ALL=(ALL) NOPASSWD: ALL" | tee -a /etc/sudoers

USER poed
WORKDIR /home/poed
CMD "/bin/bash"

ADD ../scripts /usr/local/bin
RUN /usr/local/bin/gpw_yocto_prepare
RUN /usr/local/bin/gpw_perl_prepare

RUN echo "PATH=/usr/local/bin:$PATH" | tee -a ~poed/.bashrc
RUN echo "source ~/gpw-community-bsp/sources/poky/scripts/oe-init-bashrc" | tee -a ~poed/.bashrc
RUN echo "source ~/perl5/perlbrew/etc/bashrc" | tee -a ~poed/.bashrc

# EOF
