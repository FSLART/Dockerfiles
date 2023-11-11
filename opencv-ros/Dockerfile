FROM ros:humble-ros-core

ENV DEBIAN_FRONTEND=noninteractive

# install utilities
RUN apt update
RUN apt install git build-essential wget unzip -y

# install opencv

WORKDIR /tmp/opencv
RUN wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
RUN unzip opencv.zip
WORKDIR /tmp/opencv/opencv-4.x/build
RUN cmake ..
RUN make -j8
RUN make install

# remove the code
WORKDIR /tmp
RUN rm -rf opencv