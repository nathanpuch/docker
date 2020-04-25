FROM ubuntu:18.04

RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y \
    libmodern-perl-perl libxml-libxml-perl curl build-essential 

RUN perl -MCPAN -e 'install String::LCSS'

RUN curl -OL https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2019q4/gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2
RUN mv gcc* /usr/local
RUN tar -xvf /usr/local/gcc*
RUN mv gcc* /usr/local/gcc-arm-none-eabi

RUN mkdir /etc/udev

RUN mkdir /etc/udev/rules.d

COPY 49-black_magic_probe.rules /etc/udev/rules.d
