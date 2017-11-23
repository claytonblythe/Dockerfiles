FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

MAINTAINER Clayton Blythe

## Start as root
USER root

## Setup for running behind proxy
ENV http_proxy="http://19.12.1.40:83"
ENV https_proxy="https://19.12.1.40:83"

## Download OS/Container dependencies
ENV DEBIAN_FRONTEND noninteractive
RUN apt update && apt install -y build-essential curl nano wget git && apt clean 

## Set up environment variables
ENV DEFAULT_USER cblythe2
ENV HOME /home/$DEFAULT_USER
ENV CONDA /opt/conda
ENV PATH $CONDA/bin:$PATH

# Ports to expose
EXPOSE 8097

## Create default user and home directory
RUN useradd --create-home --home-dir /home/$DEFAULT_USER --shell /bin/bash $DEFAULT_USER && \
    adduser $DEFAULT_USER sudo && \
    mkdir -p $CONDA && \
    chown $DEFAULT_USER $CONDA

## Install Python
RUN apt update && apt install -y python3-pip python-dev ipython ipython-notebook && apt clean

# Start container session with default user and default home directory
USER $DEFAULT_USER 

## INTSALL MINICONDA3 AS USER
RUN cd /tmp && \
    mkdir -p $CONDA && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
    /bin/bash Miniconda3-latest-Linux-x86_64.sh -f -b -p $CONDA && \
    rm Miniconda3-latest-Linux-x86_64.sh && \
    $CONDA/bin/conda config --system --add channels conda-forge && \
    $CONDA/bin/conda config --system --set auto_update_conda false && \
    conda install --quiet --yes conda-build && \
    conda clean -tipsy

# Install Jupyter Notebook
RUN conda install --quiet --yes 'notebook=4.3*' && \
    conda clean -tipsy

# Install scikit-learn and matplotlib and cycler 
RUN conda install --quiet --yes scikit-learn matplotlib cycler&& \
    conda clean -tipsy

# Install pytorch
RUN conda install --quiet --yes pytorch torchvision cuda80 -c soumith && \
    conda clean -tipsy 

# Create a shared volume for notebooks, scripts and data 
RUN mkdir -p /home/$DEFAULT_USER/notebooks
VOLUME /home/$DEFAULT_USER/notebooks/
WORKDIR /home/$DEFAULT_USER

# Make github directory and clone relevant repos
RUN mkdir -p ~/github && cd ~/github && git clone http://github.com/claytonblythe/pytorchNotebooks.git && git clone http://github.com/claytonblythe/deepython.git && git clone http://github.com/claytonblythe/elegantdp.git && git clone http://github.com/claytonblythe/version_control.git && git clone http://github.com/claytonblythe/dotfiles.git && git clone --depth=1 http://github.com/claytonblythe/vimrc.git ~/.vim_runtime

## Change to root
USER root

# Personalize bashrc and other dotfiles, git configuration
RUN cat ~/github/dotfiles/macbook/bashrc >> /home/$DEFAULT_USER/.bashrc && cat ~/github/dotfiles/macbook/bash_profile >> /home/$DEFAULT_USER/.bash_profile && cat ~/github/dotfiles/macbook/aliases >> /home/$DEFAULT_USER/.aliases && sh ~/.vim_runtime/install_awesome_vimrc.sh && git config --global user.email "blythec1@central.edu" && git config --global user.name "Clayton Blythe"

# Change back to default user 

# Get Sony tutorial directory 
RUN git clone https://github.com/pytorch/examples.git

# Start container session with default user and default home directory
USER $DEFAULT_USER

# run jupyter notebook
CMD jupyter notebook --no-browser --ip=0.0.0.0 --NotebookApp.token='hellofriend'
