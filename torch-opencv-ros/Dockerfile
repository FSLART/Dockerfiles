FROM fslart/opencv-cuda-ros

# update system and install wget and unzip
RUN apt update && apt upgrade -y
RUN apt install wget unzip -y

# set up the MKL repository
RUN wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB | gpg --dearmor | tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null
# add the MKL repository
RUN echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | tee /etc/apt/sources.list.d/oneAPI.list
RUN apt update
# install the MKL packages
RUN apt install intel-oneapi-mkl python3 python3-pip -y
RUN pip3 install mkl mkl-include

# install libtorch
WORKDIR /temp
RUN wget https://download.pytorch.org/libtorch/cu118/libtorch-cxx11-abi-shared-with-deps-2.1.0%2Bcu118.zip
RUN unzip libtorch-cxx11-abi-shared-with-deps-2.1.0+cu118.zip
RUN mv libtorch /usr/local/lib/libtorch
ENV LD_LIBRARY_PATH=/usr/local/lib/libtorch/lib:$LD_LIBRARY_PATH
WORKDIR /home/fslart
RUN rm -rf /temp
