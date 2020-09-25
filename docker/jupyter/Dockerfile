FROM ubuntu:16.04

ENV PYTHON_VERSION 3.6.9
ENV HOME /root
# PYTHON
ENV PYTHON_ROOT $HOME/local/bin/python-$PYTHON_VERSION
ENV PATH $PYTHON_ROOT/bin:$PATH
ENV PYENV_ROOT $HOME/.pyenv
# Jupyter Notebook config
ENV JUPYTER_CONFIG /root/.jupyter/jupyter_notebook_config.py

ENV WORK_DIR /notebook

RUN apt-get update && apt-get upgrade -y \
 && apt-get install -y tzdata

# timezone setting
ENV TZ=Asia/Tokyo
RUN apt-get install -y tzdata \
    build-essential \
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
 && apt-get autoremove -y && apt-get clean \
 && git clone https://github.com/pyenv/pyenv.git $PYENV_ROOT \
 && $PYENV_ROOT/plugins/python-build/install.sh \
 && /usr/local/bin/python-build -v $PYTHON_VERSION $PYTHON_ROOT \
 && rm -rf $PYENV_ROOT

# for librosa
RUN apt-get install -y libsndfile1-dev

# install additional packages
COPY ./requirements.txt requirements.txt
RUN pip install -U pip && pip install -r requirements.txt

# Setting for npbdaa. See issues at https://github.com/RyoOzaki/npbdaa/issues
RUN apt install gcc \
 && pip install numpy future six \
 && pip install cython \
 && git clone https://github.com/mattjj/pybasicbayes \
 && cd pybasicbayes && python setup.py install \
 && cd - && rm -rf pybasicbayes \
 && git clone https://github.com/kishiyamat/pyhsmm \
 && cd pyhsmm && python setup.py install \
 && cd - && rm -rf pyhsmm \
 && git clone https://github.com/kishiyamat/npbdaa \
 && cd npbdaa && python setup.py install \
 && cd - && rm -rf npbdaa

# Jupyter setting
RUN jupyter contrib nbextension install --user \
 && jupyter nbextensions_configurator enable --user \
 && jupyter notebook --generate-config --allow-root

# Vim for Jupyter (If you dont use Vim, coment out the following RUN)
RUN mkdir -p $(jupyter --data-dir)/nbextensions \
 && cd $(jupyter --data-dir)/nbextensions \
 && git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding \
 && jupyter nbextension enable vim_binding/vim_binding \
 && cd -

RUN echo "c.NotebookApp.ip = '0.0.0.0'" >> ${JUPYTER_CONFIG} \
 && echo "c.NotebookApp.port = 8000" >> ${JUPYTER_CONFIG} \
 && echo "c.NotebookApp.open_browser = False" >> ${JUPYTER_CONFIG} \
 && echo "c.NotebookApp.allow_root = True" >> ${JUPYTER_CONFIG} \
 && echo "c.NotebookApp.token = ''" >> ${JUPYTER_CONFIG} \
 && echo "c.NotebookApp.iopub_data_rate_limit=10000000000" >> ${JUPYTER_CONFIG} \
 && echo "c.MultiKernelManager.default_kernel_name = 'python3.6'" >> ${JUPYTER_CONFIG} \
 && echo "c.IPKernelApp.pylab = 'inline'" >> ${JUPYTER_CONFIG} \
 && echo "c.InlineBackend.figure_formats = {'png', 'retina'}" >> ${JUPYTER_CONFIG}
