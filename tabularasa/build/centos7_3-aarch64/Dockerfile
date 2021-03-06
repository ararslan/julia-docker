## This file was autogenerated
# Do not edit directly; edit the .harbor files
#
# To build this docker image via `make`, run `make build-centos7_3-aarch64` in the `tabularasa` directory
# To build this docker image manually, run `docker build --pull -t staticfloat/julia_tabularasa:centos7_3-aarch64 .`

FROM centos/aarch64




MAINTAINER Elliot Saba <staticfloat@gmail.com>
USER root

# We create a `buildworker` user so that we don't have to run everything as root
RUN useradd -m -s /bin/bash buildworker

# This is where we'll do all our work, so make it now
RUN mkdir -p /src
RUN chown buildworker:buildworker /src
RUN yum update -y && yum install -y python python-devel curl gcc gcc-c++ make
RUN curl -L 'https://bootstrap.pypa.io/get-pip.py' | python

# This enables qemu-*-static emulation on x86_64
ARG QEMU_ARCH=aarch64
USER root
# Install binfmt-support
RUN [ -z $(which apt-get 2>/dev/null) ] || apt-get install -y binfmt-support

# Download latest qemu-user-static releases
ARG QEMU_VER=2.8.3
RUN curl -L https://github.com/multiarch/qemu-user-static/releases/download/v${QEMU_VER}/qemu-${QEMU_ARCH}-static -o /usr/bin/qemu-${QEMU_ARCH}-static
RUN chmod +x /usr/bin/qemu-${QEMU_ARCH}-static

USER root

# We need to override ld.so.conf to search /usr/local before /usr
RUN echo "/usr/local/lib64" > /etc/ld.so.conf.new; \
    echo "/usr/local/lib" >> /etc/ld.so.conf.new; \
    cat /etc/ld.so.conf >> /etc/ld.so.conf.new; \
    mv /etc/ld.so.conf.new /etc/ld.so.conf; \
    ldconfig

# These are our final words. Always.
WORKDIR /

# Use /entrypoint.sh hack to get around the whole ${L32} ARG not being used in ENTRYPOINT thing.
RUN echo "#!/bin/bash" > /entrypoint.sh; echo "${L32} \"\$@\"" >> /entrypoint.sh; chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["/bin/bash"]
USER buildworker
