FROM debian:8.5

MAINTAINER Clayton Blythe <claytondblythe@gmail.com>

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

ENV http_proxy="http://19.12.1.40:83"
ENV https_proxy="https://19.12.1.40:83"

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git vim

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-4.3.14-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh

RUN apt-get install -y curl grep sed dpkg build-essential && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean


RUN mkdir -p ~/github && cd ~/github && git clone http://github.com/claytonblythe/deepython.git && git clone http://github.com/claytonblythe/elegantdp.git && git clone http://github.com/claytonblythe/version_control.git && git clone http://github.com/claytonblythe/dotfiles.git && git clone --depth=1 http://github.com/claytonblythe/vimrc.git ~/.vim_runtime

ENV PATH /opt/conda/bin:$PATH

RUN pip install jupyter pelican markdown

ENTRYPOINT [ "/usr/bin/tini", "--" ]

RUN cat ~/github/dotfiles/macbook/bashrc >> /root/.bashrc && cat ~/github/dotfiles/macbook/bash_profile >> /root/.bash_profile && cat ~/github/dotfiles/macbook/aliases >> /root/.aliases && sh ~/.vim_runtime/install_awesome_vimrc.sh && git config --global user.email "blythec1@central.edu" && git config --global user.name "Clayton Blythe"

CMD [ "/bin/bash" ]

