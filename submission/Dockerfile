# Definition of Submission container

# We start from tensorflow-gpu image
FROM tensorflow/tensorflow:1.13.2-gpu-py3

# this line prevents interaction with tzdata package
ENV DEBIAN_FRONTEND=noninteractive
# from https://github.com/tensorflow/tensorflow/issues/10776
# this may not be needed if you use pytorch image
RUN cd /usr/local/cuda/lib64 \
    && mv stubs/libcuda.so ./ \
    && ln -s libcuda.so libcuda.so.1 \
    && ldconfig


# DO NOT MODIFY: your submission won't run if you do
RUN apt-get update -y && apt-get install -y --no-install-recommends \
         gcc \
         libc-dev\
         git \
         bzip2 \
         python-tk && \
     rm -rf /var/lib/apt/lists/*

# let's create our workspace, we don't want to clutter the container
RUN mkdir /workspace
# we make the workspace our working directory
WORKDIR /workspace

# here, we install the requirements, some requirements come by default
# you can add more if you need to in requirements.txt

ARG PIP_INDEX_URL
ENV PIP_INDEX_URL=${PIP_INDEX_URL}
RUN echo PIP_INDEX_URL=${PIP_INDEX_URL}

RUN pip install -U "pip>=20.2"
COPY requirements.* ./
RUN cat requirements.* > .requirements.txt
RUN  pip3 install --use-feature=2020-resolver -r .requirements.txt




# let's copy all our solution files to our workspace
# if you have more file use the COPY command to move them to the workspace
COPY solution.py /workspace
COPY tf_models /workspace/tf_models
COPY model.py /workspace


RUN PYTHONPATH=. python -c "from solution import *"


CMD python solution.py
