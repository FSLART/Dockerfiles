FROM ros:humble-ros-base

ENV DEBIAN_FRONTEND=noninteractive

# install utilities
RUN apt update
RUN apt install git build-essential wget unzip libssl-dev libeigen3-dev libcgal-dev -y

# install opencv
WORKDIR /tmp/opencv
RUN wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
RUN unzip opencv.zip
WORKDIR /tmp/opencv/opencv-4.x/build
RUN cmake ..
RUN make -j8
RUN make install
WORKDIR /tmp
RUN rm -rf opencv

# install cmake from source
WORKDIR /temp
RUN wget https://github.com/Kitware/CMake/releases/download/v3.27.8/cmake-3.27.8.tar.gz
RUN tar -xvf cmake-3.27.8.tar.gz
WORKDIR /temp/cmake-3.27.8
RUN ./bootstrap
RUN make -j8
RUN make install
WORKDIR /home/fslart
RUN rm -rf /temp

# install lart_common
WORKDIR /temp
RUN git clone https://github.com/FSLART/lart_common.git
WORKDIR /temp/lart_common
RUN mkdir build
WORKDIR /temp/lart_common/build
RUN cmake ..
RUN make -j8
RUN make install
WORKDIR /home/fslart
RUN rm -rf /temp
