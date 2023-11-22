FROM nvidia/cuda:12.2.2-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# enable ubuntu universe repository
RUN apt update
RUN apt install software-properties-common -y
RUN apt-add-repository universe

# add GPG key
RUN apt update && apt install curl wget libssl-dev libeigen3-dev libcgal-dev -y
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

# add repository to sources list
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# upgrade the system
RUN apt update
RUN apt upgrade -y

# install ROS
RUN apt install ros-humble-ros-base -y

# setup environment libraries
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
ENV PATH=/usr/local/cuda/bin:$PATH

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
