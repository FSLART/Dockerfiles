FROM fslart/opencv-cuda-ros

# install libtorch
WORKDIR /temp
RUN wget https://download.pytorch.org/libtorch/cu118/libtorch-cxx11-abi-shared-with-deps-2.1.0%2Bcu118.zip
RUN unzip libtorch-cxx11-abi-shared-with-deps-2.1.0+cu118.zip
RUN mv libtorch /usr/local/lib/libtorch
ENV LD_LIBRARY_PATH=/usr/local/lib/libtorch/lib:$LD_LIBRARY_PATH
WORKDIR /home/fslart
RUN rm -rf /temp
