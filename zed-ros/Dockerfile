FROM stereolabs/zed:4.1-runtime-cuda11.8-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive

# enable ubuntu universe repository
RUN apt update
RUN apt install software-properties-common -y
RUN apt-add-repository universe

# add GPG key
RUN apt update && apt install git curl wget libssl-dev libeigen3-dev libcgal-dev -y
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

# install the ZED ROS wrapper
RUN mkdir -p /ros2_ws/src/
WORKDIR /ros2_ws/src/
RUN git clone --recursive https://github.com/stereolabs/zed-ros2-wrapper.git
WORKDIR /ros2_ws/
RUN . /opt/ros/humble/setup.bash && rosdep install --from-paths src --ignore-src -r -y
RUN . /opt/ros/humble/setup.sh && colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --parallel-workers 8

# run the ZED ROS wrapper
CMD . /opt/ros/humble/setup.bash && . /ros2_ws/install/setup.bash && ros2 launch zed_wrapper zed_camera.launch.py camera_model:=zed2i
