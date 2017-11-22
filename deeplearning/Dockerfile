FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

MAINTAINER Clayton Blythe <claytondblythe@gmail.com>

ARG TENSORFLOW_ARCH=gpu
ARG TORCH_VERSION=latest

## Set up environment variables
ENV DEFAULT_USER cblythe2
ENV HOME /home/$DEFAULT_USER
ENV CONDA /opt/conda
ENV PATH $CONDA/bin:$PATH
EXPOSE 8097
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
		python-dev \
		python-tk \
		python3-dev \
		python3-tk \
		ant \
		default-jdk \
		doxygen \
		python-pip \
		python3-pip

## Create default user and home directory
RUN useradd --create-home --home-dir /home/$DEFAULT_USER --shell /bin/bash $DEFAULT_USER && \
    adduser $DEFAULT_USER sudo && \
    mkdir -p $CONDA && \
    chown $DEFAULT_USER $CONDA && \
    pip3 --no-cache-dir install \
    pyopenssl ndg-httpsclient pyasn1

# Install useful Python packages using apt-get
RUN apt-get update && apt-get install -y \
		python3-numpy \
		python3-scipy \
		python3-nose \
		python3-h5py \
		python3-skimage \
		python3-matplotlib \
		python3-pandas \
		python3-sklearn \
		python3-sympy \
		&& \
	apt-get clean && \
	apt-get autoremove && \
	rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip install --upgrade pip

# Install other useful Python packages using pip3
RUN pip3 --no-cache-dir install --upgrade ipython && \
	pip3 --no-cache-dir install \
		jupyter \
		path.py \
		Pillow \
		pygments \
		six \
		sphinx \
		wheel \
		zmq

# Install TensorFlow from pip
RUN pip3 --no-cache-dir install tensorflow-gpu

# Install PyTorch
RUN pip3 install http://download.pytorch.org/whl/cu80/torch-0.2.0.post3-cp35-cp35m-manylinux1_x86_64.whl
RUN pip3 install torchvision

# Install OpenCV
RUN pip3 install opencv-python

USER $DEFAULT_USER

# Install MINICONDA3 AS USER
RUN cd /tmp && \
    mkdir -p $CONDA && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    /bin/bash Miniconda3-latest-Linux-x86_64.sh -f -b -p $CONDA && \
    rm Miniconda3-latest-Linux-x86_64.sh && \
    $CONDA/bin/conda config --system --add channels conda-forge && \
    $CONDA/bin/conda config --system --set auto_update_conda false && \
    conda install --quiet --yes conda-build && \
    conda clean -tipsy

# Make github directory and clone relevant repos
RUN mkdir -p ~/github && mkdir ~/data && cd ~/github && git clone https://github.com/claytonblythe/version_control.git && git clone http://github.com/claytonblythe/dotfiles.git && git clone --depth=1 http://github.com/claytonblythe/vimrc.git ~/.vim_runtime && git clone https://github.com/claytonblythe/dockerfiles.git

# Personalize bashrc and other dotfiles, git configuration
RUN cat ~/github/dotfiles/macbook/bashrc >> /home/$DEFAULT_USER/.bashrc && cat ~/github/dotfiles/macbook/bash_profile >> /home/$DEFAULT_USER/.bash_profile && cat ~/github/dotfiles/macbook/aliases >> /home/$DEFAULT_USER/.aliases && sh ~/.vim_runtime/install_awesome_vimrc.sh && git config --global user.email "blythec1@central.edu" && git config --global user.name "Clayton Blythe"

# Expose Ports for TensorBoard (6006), Ipython (8888)
EXPOSE 6006 8888
CMD ["/bin/bash"]
