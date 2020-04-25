FROM ubuntu:20.04
# based on paparazziuav/pprz-dep Dockerfile

# install some extra convenience packages
RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y \
    light-themes

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    terminator

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    vim

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    software-properties-common

RUN DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y \
    wget \
    gpg

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV PULSE_SERVER /run/pulse/native

# add basic user
ENV USERNAME=compx11 USER_ID=1000 USERGROUPS=sudo,dialout,plugdev
RUN useradd --shell /bin/bash -u $USER_ID -o -c "compx11 Docker user" -m $USERNAME \
    && usermod -aG $USERGROUPS $USERNAME

# set gtk theme
RUN echo include \"/usr/share/themes/Ambiance/gtk-2.0/gtkrc\" > /home/$USERNAME/.gtkrc-2.0

# handle permissions for docker volumes by dynamically changing the id of user pprz to LOCAL_USER_ID (default 1000)
# this uses https://github.com/tianon/gosu/
ENV GOSU_VERSION 1.9
ENV archi amd64

RUN set -x \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$archi" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$archi.asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -r /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true

# Add Tini
ENV TINI_VERSION v0.8.4
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini


ENTRYPOINT ["/tini", "--"]
CMD ["bash"]
