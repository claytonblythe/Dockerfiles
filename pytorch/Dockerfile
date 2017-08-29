
# Pyroch Dockerfile
# Maintainer: Clayton Blythe

FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

ENV http_proxy="http://19.12.1.40:83"
ENV https_proxy="https://19.12.1.40:83"

RUN apt-get update
RUN apt-get install -y build-essential apt-utils

RUN apt-get install -y cmake git vim libgtk2.0-dev pkg-config libavcodec-dev \
  libavformat-dev libswscale-dev
RUN  apt-get update && apt-get install -y python-dev python-numpy \
  python3 python3-pip python3-setuptools python3-dev libtbb2 libtbb-dev \
  libjpeg-dev libjasper-dev libdc1394-22-dev \
  python-opencv libopencv-dev libav-tools python-pycurl \
  libatlas-base-dev gfortran webp qt5-default libvtk6-dev zlib1g-dev

RUN pip3 install numpy

RUN apt-get install -y python-pip
RUN pip install --upgrade pip

RUN pip3 install scipy
RUN pip3 install tensorflow
RUN pip3 install http://download.pytorch.org/whl/cu80/torch-0.1.12.post2-cp35-cp35m-linux_x86_64.whl
RUN pip3 install torchvision

RUN cd ~/ &&\
    git clone https://github.com/Itseez/opencv.git &&\
    git clone https://github.com/Itseez/opencv_contrib.git &&\
    cd opencv && mkdir build && cd build && cmake  -DWITH_QT=ON -DWITH_OPENGL=ON -DFORCE_VTK=ON -DWITH_TBB=ON -DWITH_GDAL=ON -DWITH_XINE=ON -DBUILD_EXAMPLES=ON .. && \
    make -j8 && make install && ldconfig && rm -rf ~/opencv*  # Remove the opencv folders to reduce image size

#
