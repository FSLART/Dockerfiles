FROM nvidia/cuda:12.1.1-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install ROS

# enable ubuntu universe repository
RUN apt update && apt upgrade -y
RUN apt install software-properties-common build-essential -y
RUN apt-add-repository universe

# add GPG key
RUN apt update && apt install git curl wget libssl-dev -y
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

# add repository to sources list
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# install ROS
RUN apt update
RUN apt install ros-humble-ros-base -y

# Update the system
RUN apt-get update && apt-get upgrade -y

# Install ROS dependencies
RUN apt-get install build-essential python3-rosdep python3-colcon-common-extensions python3-rosinstall-generator python3-vcstool python3-rosinstall -y

# setup environment variables
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
ENV PATH=/usr/local/cuda/bin:$PATH

# Install ZED SDK
WORKDIR /opt
RUN apt-get install wget zstd -y
RUN wget https://download.stereolabs.com/zedsdk/4.0/cu121/ubuntu22 -O zed_sdk.run
RUN chmod +x zed_sdk.run
RUN ./zed_sdk.run silent skip_tools skip_cuda
RUN rm zed_sdk.run

# Update the system and install the CUDA toolkit
ENV NVIDIA_DRIVER_CAPABILITIES video,compute,utility
RUN apt-get update && apt upgrade -y

# Install ZED ROS Wrapper
WORKDIR /zed-ros
RUN mkdir -p /zed-ros/src
WORKDIR /zed-ros/src
RUN git clone -b humble-v4.0.8 --recursive https://github.com/FSLART/zed-ros2-wrapper.git
WORKDIR /zed-ros
RUN rosdep init && rosdep update
RUN rosdep install --from-paths src --ignore-src -r -y
RUN apt-get install -y ros-humble-nmea-msgs \
    ros-humble-geographic-msgs \
    ros-humble-image-transport \
    ros-humble-image-transport-plugins \
    ros-humble-diagnostic-updater \
    ros-humble-robot-localization \
    ros-humble-xacro
RUN pip3 install argcomplete \
    numpy \
    empy \
    lark
