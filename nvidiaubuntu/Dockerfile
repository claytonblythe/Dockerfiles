FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

ENV http_proxy="http://19.12.1.40:83"
ENV https_proxy="https://19.12.1.40:83"

# Install some dependencies
RUN apt-get update && apt-get install -y \
                bc \
                build-essential \
                cmake \
                curl \
                g++ \
                gfortran \
                git \
                pkg-config \
                unzip \
                vim \
                wget \
                python3-dev \
                python3-tk \
                ant \
                default-jdk \
                doxygen \
                python3-pip \
                yasm \
                libswscale-dev\
                libtbb2 \
                libtbb-dev \
                libjpeg-dev \
                libpng-dev \
                libtiff-dev \
                libjasper-dev \
                libavformat-dev \
                libpq-dev  

# Prequisite for OpenCV
RUN pip3 install numpy

# Install OpenCV
WORKDIR /
RUN wget https://github.com/opencv/opencv/archive/3.3.0.zip \
&& unzip 3.3.0.zip \
&& mkdir /opencv-3.3.0/cmake_binary \
&& cd /opencv-3.3.0/cmake_binary \
&& cmake -DBUILD_TIFF=ON \
  -DBUILD_opencv_java=OFF \
  -DWITH_CUDA=OFF \
  -DENABLE_AVX=ON \
  -DWITH_OPENGL=ON \
  -DWITH_OPENCL=ON \
  -DWITH_IPP=ON \
  -DWITH_TBB=ON \
  -DWITH_EIGEN=ON \
  -DWITH_V4L=ON \
  -DBUILD_TESTS=OFF \
  -DBUILD_PERF_TESTS=OFF \
  -DCMAKE_BUILD_TYPE=RELEASE \
  -DCMAKE_INSTALL_PREFIX=$(python3.5 -c "import sys; print(sys.prefix)") \
  -DPYTHON_EXECUTABLE=$(which python3.6) \
  -DPYTHON_INCLUDE_DIR=$(python3.5 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
  -DPYTHON_PACKAGES_PATH=$(python3.5 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") .. \
&& make install \
&& rm /3.3.0.zip \
&& rm -r /opencv-3.3.0

# Install TensorFlow from pip
RUN pip3 --no-cache-dir install tensorflow-gpu

# Install PyTorch
RUN pip3 install http://download.pytorch.org/whl/cu80/torch-0.2.0.post3-cp35-cp35m-manylinux1_x86_64.whl torchvision

