FROM ubuntu:trusty

ARG QT=5.6.1
ARG QTM=5.6

RUN apt-get update -q && \
    DEBIAN_FRONTEND=noninteractive apt-get install -q -y --no-install-recommends \
        ant \
        build-essential \
        ca-certificates \
        default-jdk \
        git \
        libfontconfig1 \
        libice6 \
        libsm6 \
        libX11-xcb1 \
        libxext6 \
        libxrender1 \
        openssh-client \
        p7zip \
        xvfb \
    && apt-get clean

ADD http://download.qt.io/official_releases/qt/${QTM}/${QT}/qt-opensource-linux-x64-${QT}.run /tmp/qt/installer.run
ADD qt-installer-noninteractive.qs /tmp/qt/script.qs

RUN chmod +x /tmp/qt/installer.run \
    && xvfb-run /tmp/qt/installer.run --script /tmp/qt/script.qs \
     | egrep -v '\[[0-9]+\] Warning: (Unsupported screen format)|((QPainter|QWidget))' \
    && rm -rf /tmp/qt

RUN echo /opt/qt/${QTM}/gcc_64/lib > /etc/ld.so.conf.d/qt-${QTM}.conf
RUN locale-gen en_US.UTF-8 && DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales

RUN apt-get install -y build-essential gdb wget python debhelper ccache git gcc-multilib && \
apt-get install -y zlib1g-dev libusb-1.0-0-dev libudev-dev libgl1-mesa-dev && \
apt-get install -y libfontconfig

RUN git clone git://github.com/d-ronin/dRonin.git
RUN cd dRonin && make arm_sdk_install

RUN ln -s /opt/qt /dRonin/tools/Qt5.6.1

RUN git clone https://github.com/raspberrypi/tools /pitools
ENV PATH "$PATH:/pitools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian/bin"
