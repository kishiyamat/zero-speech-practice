# FROM continuumio/miniconda3
FROM continuumio/anaconda3

RUN apt-get update && apt-get upgrade -y

# timezone setting
ENV TZ=Asia/Tokyo
RUN apt-get install -y tzdata \
    build-essential \
    gcc \
    curl \
    git \
    libbz2-dev \
    libcurl4-openssl-dev \
    libffi-dev \
    liblzma-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libpq-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    libxml2-dev \
    llvm \
    make \
    r-base \
    tk-dev \
    unzip \
    vim \
    wget \
    xz-utils \
    zlib1g-dev \
 && apt-get autoremove -y && apt-get clean

# RUN conda update -n base -c defaults conda -y
RUN git clone https://github.com/kishiyamat/zerospeech2020.git \
 && cd zerospeech2020 \
 && conda env create -n zerospeech2020 -f environment.yaml

# without the second line the following error occurs
# See: https://github.com/conda/conda/issues/7980
# CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
# To initialize your shell, run
#     $ conda init <SHELL_NAME>
RUN cd zerospeech2020 \
 && eval "$(conda shell.bash hook)" \
 && conda activate zerospeech2020 \
 && python setup.py install \
 && conda install git pip \
 && pip install git+https://github.com/bootphon/h5features.git@master \
 && pip install git+https://github.com/bootphon/ABXpy.git@master \
 && pip install git+https://github.com/bootphon/tdev2.git@master \
 && cd -

RUN rm -rf zerospeech2020
