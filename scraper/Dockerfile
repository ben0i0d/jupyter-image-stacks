FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:python-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
# install system dependencies
RUN apt-get update && apt-get -y install gcc make gnupg2 
#add chrome source
RUN wget -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list' && \
# install chrome
    apt-get update && apt-get -y installgoogle-chrome-stable && \
    apt-get clean && rm -rf /var/lib/apt/lists/* && \
# install chromedriver
    wget -O /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`/chromedriver_linux64.zip  && \
    unzip /tmp/chromedriver.zip chromedriver -d /usr/local/bin/ 

USER ${NB_UID}

# Install Python 3 packages
RUN mamba install --quiet --yes 'beautifulsoup4' 'requests' 'selenium' 'schedule' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

USER ${NB_UID}
