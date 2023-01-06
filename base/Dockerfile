FROM debian:bullseye-slim

# PRE
# ---------

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -qq update -y
RUN apt-get install -yq apt-utils >/dev/null 2>&1 

# SSH 
# ---------

RUN apt-get install -y \
  openssh-server \
  > /dev/null

RUN mkdir /var/run/sshd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]

# MISC PACKAGES
# ---------

RUN apt-get install -y \
  curl \
  git \
  vim-nox \
  bash-completion \
  wget \
  boxes \
  > /dev/null


# VIM PLUGINS
# ---------

# pathogen

RUN mkdir -p ~/.vim/autoload ~/.vim/bundle && \
     curl -LSso ~/.vim/autoload/pathogen.vim \
     https://tpo.pe/pathogen.vim

# nerdtree

RUN cd ~/.vim/bundle && \
    git clone https://github.com/scrooloose/nerdtree

# youcompleteme

RUN apt-get -y install \
     build-essential \
     cmake \
     mono-complete \
     golang \
     nodejs \
     default-jdk \
     python3-dev \
     npm \
     > /dev/null

RUN cd ~/.vim/bundle && \ 
    git clone https://github.com/valloric/youcompleteme

RUN cd ~/.vim/bundle/youcompleteme && \
  git submodule update --init --recursive

RUN cd ~/.vim/bundle/youcompleteme && \
    python3 install.py --all --verbose --force-sudo

# vim-airline

RUN cd ~/.vim/bundle && \
	git clone https://github.com/vim-airline/vim-airline

# vim-airline-themes

RUN cd ~/.vim/bundle && \
	git clone https://github.com/vim-airline/vim-airline-themes

# vim misc

RUN mkdir ~/.vim/vim_swap
COPY vimrc /root/.vim/vimrc

# POST 
# ---------

# bash completion

RUN echo "source /etc/profile.d/bash_completion.sh" >> ~/.bashrc

# docker

RUN curl -fsSL https://get.docker.com -o get-docker.sh && \
	sh get-docker.sh

#ARG ROOT_PASSWORD=123
#RUN echo "root:${ROOT_PASSWORD}" | chpasswd

RUN sed -i 's/#PermitRootLogin.*$/PermitRootLogin yes/g' /etc/ssh/sshd_config

VOLUME "/root/"
WORKDIR "/root/"

LABEL org.opencontainers.image.source="https://github.com/igor-sadza/dev-environment"
