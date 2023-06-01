FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c
LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
COPY ijava-1.3.0.zip ./ijava-kernel.zip
# Install dependencies
RUN apt-get update && apt-get install -y gnupg && \
  curl -s https://repos.azul.com/azul-repo.key | sudo gpg --dearmor -o /usr/share/keyrings/azul.gpg && \
  echo "deb [signed-by=/usr/share/keyrings/azul.gpg] https://repos.azul.com/zulu/deb stable main" | sudo tee /etc/apt/sources.list.d/zulu.list && \
# Install Zulu OpenJdk 11 (LTS)
  apt update && apt install -y zulu11-jdk && \
  apt-get clean && rm -rf /var/lib/apt/lists/*  && \
# Unpack and install the kernel
  unzip ijava-kernel.zip -d ijava-kernel  && \
  cd ijava-kernel  && \
  python3 install.py --sys-prefix && \
# Install jupyter RISE extension.
  pip install jupyter_contrib-nbextensions RISE  && \
  jupyter-nbextension install rise --py --system  && \
  jupyter-nbextension enable rise --py --system  && \
  jupyter contrib nbextension install --system  && \
  jupyter nbextension enable hide_input/main && \
# Cleanup
  rm ../ijava-kernel.zip
# Set user back to priviledged user.
USER $NB_USER