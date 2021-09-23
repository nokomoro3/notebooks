FROM nvidia/cuda:11.2.0-cudnn8-runtime-ubuntu20.04

WORKDIR "/tmp"

ENV LANG C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

RUN apt update && \
    apt upgrade -y

RUN apt install -y \
    wget \
    libboost-all-dev \
    cmake \
    check \
    git \
    libffi-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    liblzma-dev \
    curl

#--------------------------
# python3.8.12
#--------------------------
RUN wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tar.xz 2>/dev/null 1>/dev/null
RUN tar xJf Python-3.8.12.tar.xz && \
    cd Python-3.8.12 && \
    ./configure && \
    make && \
    make install
RUN apt purge -y python3 python
RUN python3 -m pip install --upgrade pip
RUN cd /usr/local/bin && \
    ln -s python3 python

#--------------------------
# jupyterlab
#--------------------------
RUN pip install jupyterlab==3.1.12
RUN pip install jupyterlab_widgets
RUN pip install ipywidgets
RUN jupyter nbextension enable --py widgetsnbextension

# jupyterlab/toc
RUN apt install -y nodejs npm && npm install n -g && n stable && apt purge -y nodejs npm
RUN jupyter labextension install @jupyterlab/toc

# jupyterlab/git
RUN pip install jupyterlab-git
RUN jupyter labextension install @jupyterlab/git

# tensorboard for jupyterlab3
# ref. https://github.com/chaoleili/jupyterlab_tensorboard/issues/28
RUN pip install git+https://github.com/cliffwoolley/jupyter_tensorboard.git

# Set up our notebook config.
COPY jupyter_lab_config.py /root/.jupyter/jupyter_lab_config.py

RUN rm -rf /tmp/*
RUN apt clean
# RUN apt autoremove -y

#--------------------------
# poetry
#--------------------------
RUN pip install poetry
RUN poetry config virtualenvs.create false
# RUN poetry config virtualenvs.in-project true

WORKDIR "/app"
COPY pyproject.toml pyproject.toml
RUN poetry install
RUN python3 -m ipykernel install --user --name=app

WORKDIR "/app/notebooks"
CMD ["jupyter", "lab", "--allow-root"]
