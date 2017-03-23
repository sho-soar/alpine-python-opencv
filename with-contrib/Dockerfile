FROM python:3.5-alpine

ENV SRC_DIR=/tmp
ENV CC=/usr/bin/clang CXX=/usr/bin/clang++

RUN echo http://dl-cdn.alpinelinux.org/alpine/edge/testing >> /etc/apk/repositories \
    && echo http://dl-cdn.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories \
    && apk update \

    # install build dependencies
    && apk add --no-cache --virtual .build-deps \
        build-base \
        clang \
        clang-dev \
        cmake \
        git \
        wget \
        unzip \

    # install opencv dependencies
    && apk add --no-cache \
        jasper-dev \
        libavc1394-dev  \
        libdc1394-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libtbb \
        libtbb-dev \
        libwebp-dev \
        linux-headers \
        openblas-dev \
        tiff-dev \

    # fix for numpy compilation
    && ln -s /usr/include/locale.h /usr/include/xlocale.h \

    # install numpy
    && pip install \
        numpy==1.12.0 \

    # download opencv source
    && mkdir -p ${SRC_DIR} \
    && cd ${SRC_DIR} \
    && wget https://github.com/opencv/opencv/archive/3.2.0.zip \
    && unzip 3.2.0.zip \
    && mv opencv-3.2.0 opencv \
    && rm 3.2.0.zip \

    # download opnecv_contrib source
    && wget https://github.com/opencv/opencv_contrib/archive/3.2.0.zip \
    && unzip 3.2.0.zip \
    && mv opencv_contrib-3.2.0 opencv_contrib \
    && rm 3.2.0.zip \

    # build
    && mkdir -p ${SRC_DIR}/opencv/build \
    && cd ${SRC_DIR}/opencv/build \
    && cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/ -D BUILD_DOCS=OFF .. \
    && make -j4 \
    && make install \
    && rm -rf ${SRC_DIR} \
    && ln /dev/null /dev/raw1394 \
    && apk del --purge .build-deps
