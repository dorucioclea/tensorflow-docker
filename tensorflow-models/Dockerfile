ARG UBUNTU_VERSION=18.04

FROM ubuntu:${UBUNTU_VERSION} AS base

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        git \
        libcurl3-dev \
        libfreetype6-dev \
        libhdf5-serial-dev \
        libzmq3-dev \
        pkg-config \
        rsync \
        software-properties-common \
        sudo \
        unzip \
        zip \
        zlib1g-dev \
        openjdk-8-jdk \
        openjdk-8-jre-headless \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV CI_BUILD_PYTHON python

# CACHE_STOP is used to rerun future commands, otherwise cloning tensorflow will be cached and will not pull the most recent version
ARG CACHE_STOP=1
# Check out TensorFlow source code if --build-arg CHECKOUT_TF_SRC=1
ARG CHECKOUT_TF_SRC=0
# In case of Python 2.7+ we need to add passwd entries for user and group id
RUN chmod a+w /etc/passwd /etc/group
RUN test "${CHECKOUT_TF_SRC}" -eq 1 && git clone https://github.com/tensorflow/tensorflow.git /tensorflow_src || true

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip

RUN python3 -m pip --no-cache-dir install --upgrade \
    pip \
    setuptools

# Some TF tools expect a "python" binary
RUN ln -s $(which python3) /usr/local/bin/python

RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    git \
    wget \
    openjdk-8-jdk \
    python3-dev \
    virtualenv \
    swig

RUN python3 -m pip --no-cache-dir install \
    Pillow \
    h5py \
    keras_preprocessing \
    matplotlib \
    mock \
    numpy \
    scipy \
    sklearn \
    pandas \
    future \
    portpicker \
    enum34

# Install bazel
ARG BAZEL_VERSION=3.0.0
RUN mkdir /bazel && \
    wget -O /bazel/installer.sh "https://github.com/bazelbuild/bazel/releases/download/${BAZEL_VERSION}/bazel-${BAZEL_VERSION}-installer-linux-x86_64.sh" && \
    wget -O /bazel/LICENSE.txt "https://raw.githubusercontent.com/bazelbuild/bazel/master/LICENSE" && \
    chmod +x /bazel/installer.sh && \
    /bazel/installer.sh && \
    rm -f /bazel/installer.sh

RUN python3 -m pip install --no-cache-dir jupyter matplotlib
# Pin ipykernel and nbformat; see https://github.com/ipython/ipykernel/issues/422
RUN python3 -m pip install --no-cache-dir jupyter_http_over_ws ipykernel==5.1.1 nbformat==4.4.0
RUN jupyter serverextension enable --py jupyter_http_over_ws

# RUN mkdir -p /tf/tensorflow-tutorials && chmod -R a+rwx /tf/
# RUN mkdir /.local && chmod a+rwx /.local
# RUN apt-get install -y --no-install-recommends wget

# # some examples require git to fetch dependencies
# RUN apt-get install -y --no-install-recommends git
# WORKDIR /tf/tensorflow-tutorials
# RUN wget https://raw.githubusercontent.com/tensorflow/docs/master/site/en/tutorials/keras/classification.ipynb
# RUN wget https://raw.githubusercontent.com/tensorflow/docs/master/site/en/tutorials/keras/overfit_and_underfit.ipynb
# RUN wget https://raw.githubusercontent.com/tensorflow/docs/master/site/en/tutorials/keras/regression.ipynb
# RUN wget https://raw.githubusercontent.com/tensorflow/docs/master/site/en/tutorials/keras/save_and_load.ipynb
# RUN wget https://raw.githubusercontent.com/tensorflow/docs/master/site/en/tutorials/keras/text_classification.ipynb
# RUN wget https://raw.githubusercontent.com/tensorflow/docs/master/site/en/tutorials/keras/text_classification_with_hub.ipynb
# RUN apt-get autoremove -y && apt-get remove -y wget
# WORKDIR /tf
# EXPOSE 8888

RUN python3 -m ipykernel.kernelspec


#
# Create the tensorflow models folder
#
RUN mkdir -p /tensorflow/models && chmod -R a+rwx /tensorflow/models/
RUN apt-get install -y --no-install-recommends python-pil python-lxml python-tk
RUN apt-get install -y --no-install-recommends  protobuf-compiler 

RUN python3 -m pip --no-cache-dir install Cython contextlib2 jupyter matplotlib ts_slim lxml pillow
# RUN pip install --user Cython
# RUN pip install --user contextlib2
# RUN pip install --user jupyter
# RUN pip install --user matplotlib
# RUN pip install --user ts_slim
# RUN pip install --user lxml
# RUN pip install --user pillow

RUN git clone https://github.com/tensorflow/models.git /tensorflow/models

WORKDIR /tensorflow/models/research
RUN protoc object_detection/protos/*.proto --python_out=.

RUN export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
RUN jupyter notebook --generate-config --allow-root
# RUN echo "c.NotebookApp.password = u'sha1:6a3f528eec40:6e896b6e4828f525a6e20e5411cd1c8075d68619'" >> /root/.jupyter/jupyter_notebook_config.py

EXPOSE 8888

CMD ["jupyter", "notebook", "--allow-root", "--notebook-dir=/tensorflow/models/research/object_detection", "--ip=0.0.0.0", "--port=8888", "--no-browser"]

# CMD ["jupyter", "notebook", "--allow-root", "--notebook-dir=/tf", "--ip=0.0.0.0", "--port=8888", "--no-browser"]


