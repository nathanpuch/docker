FROM ubuntu:18.04

RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y \
    libmodern-perl-perl libxml-libxml-perl curl build-essential unzip libc6-i386 openjdk-8-jre

RUN perl -MCPAN -e 'install String::LCSS'

RUN curl -OL https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2019q4/gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2
RUN mv gcc* /usr/local
RUN tar -xvf /usr/local/gcc*
RUN mv gcc* /usr/local/gcc-arm-none-eabi

RUN mkdir /etc/udev

RUN mkdir /etc/udev/rules.d

COPY 49-black_magic_probe.rules /etc/udev/rules.d

ADD en.stm32cubemx.zip /tmp/en.stm32cubemx.zip
ADD auto-install.xml /tmp/auto-install.xml

WORKDIR /tmp

RUN unzip en.stm32cubemx.zip 
RUN chmod +x /tmp/SetupSTM32CubeMX-5.6.1.linux
RUN /tmp/SetupSTM32CubeMX-5.6.1.linux auto-install.xml

ENV PATH="$PATH:/usr/local/gcc-arm-none-eabi/bin"

WORKDIR /home/code