FROM nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04

RUN apt-get update && apt-get install -y \
	wget \
	vim \
  python-wxtools \
  xvfb \
  curl \
  unzip \
	bzip2

#Install MINICONDA
RUN wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O Miniconda.sh && \
	/bin/bash Miniconda.sh -b -p /opt/conda && \
	rm Miniconda.sh
ENV PATH /opt/conda/bin:$PATH

#Install ANACONDA Environment
RUN conda create -y -n dlc python=3.6 imageio=2.3.0 numpy=1.14.5 six=1.11.0 wxPython anaconda && \
	/opt/conda/envs/dlc/bin/pip install tensorflow-gpu deeplabcut==2.0.7.2

# Setup working dir:
RUN mkdir -p /workdir
WORKDIR /workdir

ARG MODELFILE
COPY ${MODELFILE} .
RUN mkdir -p /workdir/model
RUN unzip ${MODELFILE} -d ./tmp
RUN mv ${MODELFILE} tmp/*/* model

ARG DLC_FILENAME
COPY ${DLC_FILENAME} .
RUN unzip -j ${DLC_FILENAME} -d .


# Entrypoint
CMD ["/bin/bash", "-c", "source activate dlc && python $SCRIPT $ARGS"]


# CMD ["/bin/bash", "-c", "echo $ARGS"]
# CMD ["/bin/bash", "-c", "echo python $SCRIPT $ARGS"]
# FROM continuumio/anaconda3:5.2.0

# # RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
# # RUN apt-get update -yqq \
# #   && apt-get install -yqq --no-install-recommends \
# #     netcat emacs curl apt-utils wget gnupg procps unzip sudo\
# #   && apt-get -q clean

# RUN conda install -y tensorflow-gpu=1.3.0 ipython
# RUN conda install -y pandas==0.21.0
# RUN conda install -y matplotlib==3.0.3
# RUN conda install -y numpy==1.14.5
# RUN pip install deeplabcut==2.0.7.2

# # Ingest docker-build build-args
# ARG TOKEN
# ARG RUNNER
# ARG DLC_FILENAME

# RUN mkdir -p /workdir
# WORKDIR /workdir

# COPY ${RUNNER} .
# COPY ${DLC_FILENAME} .

# RUN unzip -j ${DLC_FILENAME} -d .

# ENV _RUNNER=${RUNNER}
# CMD ["sh", "-c", "python ${_RUNNER} execute"]