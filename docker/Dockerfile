# Build a docker image for developing WCH32V003
#
# Included in the image is:
# * TODO

FROM docker.io/library/debian:12 AS base

RUN apt-get -qq update -y

# Nice to have
RUN DEBIAN_FRONTEND=noninteractive \
      apt-get install -y --no-install-recommends \
            vim \
            tmux \
            wget \
            ca-certificates \
            make \
            gdb-multiarch \
            git \
            cmake \
            build-essential \
            python3 \
            automake \
            autoconf \
            texinfo \
            libtool \
            pkg-config

ARG XPACK_VERSION=14.2.0-3

WORKDIR /riscv
RUN wget https://github.com/xpack-dev-tools/riscv-none-elf-gcc-xpack/releases/download/v$XPACK_VERSION/xpack-riscv-none-elf-gcc-$XPACK_VERSION-linux-x64.tar.gz
RUN tar -xf xpack-riscv-none-elf-gcc-$XPACK_VERSION-linux-x64.tar.gz

ENV PATH="$PATH:/riscv/xpack-riscv-none-elf-gcc-$XPACK_VERSION/bin"

RUN DEBIAN_FRONTEND=noninteractive \
      apt-get install -y --no-install-recommends \
            libudev-dev \
            libusb-1.0-0-dev 
#            libjaylink-dev

WORKDIR /riscv
#RUN git clone https://github.com/treideme/openocd-hacks.git -b mrs-wch-riscv-230824 openocd
#RUN git clone https://github.com/cjacker/wch-openocd openocd
RUN git clone https://github.com/jnk0le/openocd-wch.git -b mrs-wch-riscv-230824-LIBERATED openocd
WORKDIR /riscv/openocd

RUN ./bootstrap
RUN ./configure \
	--enable-wlinke \
	--disable-ch347 \
	--disable-linuxgpiod \
	--disable-werror

RUN make -j`nproc`
RUN make install



#RUN useradd -ms /bin/bash dev
#USER dev
