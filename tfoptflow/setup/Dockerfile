
FROM nvidia/cuda:9.0-cudnn7-devel


RUN \
  apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install --fix-missing --no-install-recommends -y \
  python2.7 \
  python-pip \
  python-setuptools \
  python-dev \
  libxml2-dev \
  libxslt-dev \
  && apt-get install -y cmake wget curl \
  && apt-get install -y libglib2.0-0 libsm6 libxrender1 libxext6 \
  && rm -rf /var/lib/apt/lists/*

RUN \
  apt-get update \
  && apt-get install ffmpeg libsm6 libxext6  -y

ENV CONDA_PATH=/opt/anaconda3

RUN cd /tmp && curl -O https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh
RUN chmod +x /tmp/Anaconda3-2020.11-Linux-x86_64.sh
RUN mkdir /root/.conda
RUN bash -c "/tmp/Anaconda3-2020.11-Linux-x86_64.sh -b -p ${CONDA_PATH}"

ENV ENVIRONMENT_NAME=dlubu36

# Initializes Conda for bash shell interaction.
RUN ${CONDA_PATH}/bin/conda init bash

# Upgrade Conda to the latest version
RUN ${CONDA_PATH}/bin/conda update -n base -c defaults conda -y

# Create the work environment and setup its activation on start.
RUN ${CONDA_PATH}/bin/conda create --name ${ENVIRONMENT_NAME} -y
RUN echo conda activate ${ENVIRONMENT_NAME} >> /root/.bashrc

COPY ./dlubu36.yml /tmp/
RUN . ${CONDA_PATH}/bin/activate ${ENVIRONMENT_NAME} \
  && conda env update --file /tmp/dlubu36.yml --prune
