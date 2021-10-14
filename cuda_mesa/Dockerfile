FROM nvidia/cuda:11.4.1-devel-ubuntu20.04

# install some needed packages and set gcc-8 as default compiler
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    llvm-7 \
    llvm-dev \
    autoconf \
    automake \
    bison \
    flex \
    gettext \
    libtool \
    python-dev\
    git \
    pkgconf \
    python-mako \
    zlib1g-dev \
    x11proto-gl-dev \
    libxext-dev \
    xcb \
    libx11-xcb-dev \
    libxcb-dri2-0-dev \
    libxcb-xfixes0-dev \
    libdrm-dev \
    g++ \
    make \
    xvfb \
    x11vnc \
    g++-8 && \
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 800 --slave /usr/bin/g++ g++ /usr/bin/g++-8

ENV GIT_SSL_NO_VERIFY=1

# get mesa (using 19.0.2 as later versions dont use the configure script)
WORKDIR /mesa
RUN git clone https://gitlab.freedesktop.org/mesa/mesa.git
WORKDIR /mesa/mesa
RUN git checkout mesa-19.0.2

# build and install mesa
RUN libtoolize && \
    autoreconf --install && \
    ./configure \
        --enable-glx=gallium-xlib \
        --with-gallium-drivers=swrast,swr \
        --disable-dri \
        --disable-gbm \
        --disable-egl \
        --enable-gallium-osmesa \
        --enable-autotools \
        --enable-llvm \
        --with-llvm-prefix=/usr/lib/llvm-7/ \
        --prefix=/usr/local && \
    make -j 4 && \
    make install && \
    rm -rf /mesa

ENV  GALLIUM_DRIVER="llvmpipe" \
    LP_DEBUG="" \
    LP_NO_RAST="false" \
    LP_NUM_THREADS="" \
    LP_PERF="" \
    MESA_VERSION="19.0.2" \
    XVFB_WHD="1920x1080x24"
