FROM nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
ENV PATH=/usr/local/cuda/bin:$PATH

# install utilities
RUN apt update
RUN apt upgrade -y
RUN apt install -y --no-install-recommends git build-essential wget unzip libssl-dev python3 python3-dev python3-setuptools python3-wheel python3-pip python3-numpy python3-packaging gdb -y

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
RUN apt purge cmake -y

# install onnx
WORKDIR /tmp/onnx
RUN git clone https://github.com/microsoft/onnxruntime.git --recursive --branch v1.16.3
WORKDIR /tmp/onnx/onnxruntime
RUN ./build.sh --config RelWithDebInfo --allow_running_as_root --update --build --build_wheel --use_cuda --cuda_home /usr/local/cuda --cudnn_home /usr/lib/x86_64-linux-gnu --build_shared_lib --build_dir /tmp/onnx/onnxruntime/build --cmake_extra_defines CMAKE_INSTALL_PREFIX=/usr/local --compile_no_warning_as_error --skip_submodule_sync
WORKDIR /tmp/onnx/onnxruntime/build/Linux/RelWithDebInfo
RUN make install
# install onnx python package
ENV ONNX_ML=1
RUN python3 setup.py bdist_wheel
RUN pip3 install --upgrade /tmp/onnx/onnxruntime/build/Linux/RelWithDebInfo/dist/*.whl

# install opencv
WORKDIR /tmp/opencv
RUN wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
RUN unzip opencv.zip
WORKDIR /tmp/opencv/opencv-4.x/build
RUN cmake -DWITH_CUDA=ON -DWITH_CUDNN=ON ..
RUN make -j8
RUN make install

# install ROS
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