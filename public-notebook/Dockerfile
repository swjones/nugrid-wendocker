FROM ubuntu:latest

MAINTAINER Samuel Jones

# scikits.audiolab needs dev stuff
# clean-up unecessary packages once installed.

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && \
    apt-get --no-install-recommends install --yes \
    ipython-notebook python-pip \
    python-numpy python-scipy python-matplotlib python-pandas python-sympy \
    python-sklearn hdf5-tools libhdf5-serial-dev python-h5py python-tk \
    build-essential python-dev libsndfile1-dev libsndfile1 git wget \
    pandoc
RUN apt-get --no-install-recommends install --yes libfreetype6-dev libpng3 
RUN pip install --upgrade matplotlib && \
    pip install scikits.audiolab && \
    pip install pygments && \
    apt-get remove --yes libsndfile-dev python-dev build-essential && \
    apt-get autoremove --yes && \
    apt-get autoclean --yes && \
    pip install --upgrade requests && \
    pip install --upgrade nugridpy vos && \
    pip install https://github.com/jakevdp/JSAnimation/archive/master.zip && \
    pip install xlrd

#RUN  pip install ipython==2.0.0 && \
RUN  pip install ipython==3.1.0 && \
     pip install jsonschema && \
     pip install jinja2 && \
     pip install tornado
RUN  pip install --upgrade tornado

# set the lang to avoid issues
RUN apt-get install -y language-pack-en
ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN locale-gen en_US.UTF-8
RUN dpkg-reconfigure locales

EXPOSE 8080
RUN useradd -d /home/nugrid -m -c "Nugrid Public User" nugrid
ADD ./run-ipython-notebook.bash /home/nugrid/
RUN  chmod +x /home/nugrid/run-ipython-notebook.bash && \ 
     mkdir -p /home/nugrid/CADC/NuGrid && \
     chown -R nugrid:nugrid /home/nugrid 
USER nugrid
ENV HOME /home/nugrid
RUN ipython profile create nbserver
# startup.ipy moved to https://github.com/NuGrid/WENDI/
ADD nb_tools.py /home/nugrid/.ipython/profile_nbserver/startup/nb_tools.py
WORKDIR /home/nugrid
CMD timeout 43200 /home/nugrid/run-ipython-notebook.bash # 12 hours
