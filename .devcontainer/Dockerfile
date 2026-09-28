FROM pytorch/pytorch:2.14.0-cuda13.0-cudnn9-devel

ARG USERNAME="vscode"
ARG DEVELOPER_HOME="/home/${USERNAME}"

VOLUME ["/models"]

# Install updates
RUN apt update && apt upgrade -y
RUN apt install -y git
RUN apt clean

# Install Anaconda
RUN wget https://repo.anaconda.com/archive/Anaconda3-2026.07-1-Linux-x86_64.sh -O anaconda.sh -q && \
  /bin/bash anaconda.sh -b -p /opt/conda && \
  rm anaconda.sh && \
  ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
  echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
  echo "conda activate" >> ~/.bashrc && \
  find /opt/conda/ -follow -type f -name '*.a' -delete && \
  find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
  /opt/conda/bin/conda clean -afy

# Init conda and create environment
RUN . ~/.bashrc && \
  conda init --all && \
  conda config --set plugins.auto_accept_tos yes && \
  conda config --set auto_activate_base false && \
  conda create -n tripoSG python=3.10 && \
  echo "conda activate tripoSG" >> ~/.bashrc

# Update pip
RUN . ~/.bashrc && \
  python -m pip install -q --upgrade pip

# Install requirements
COPY ./requirements.txt ${DEVELOPER_HOME}/workspace/
RUN . ~/.bashrc && \
  pip install -q -r ${DEVELOPER_HOME}/workspace/requirements.txt

# pip install diso --no-build-isolation
