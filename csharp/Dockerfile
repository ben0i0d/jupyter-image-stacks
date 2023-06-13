FROM eoelab.org:1032/build-image-stacks/jupyter-image-stacks/jupyter:up-c

LABEL maintainer="eoelab <eoelab@eoelab.org>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install Mono 
USER root
RUN apt-get update && apt-get install -yq --no-install-recommends gnupg && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    echo "deb https://download.mono-project.com/repo/ubuntu stable-focal main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list && \
    apt-get update && apt-get install -yq --no-install-recommends mono-complete && \
    mozroots --import --machine --sync && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
RUN cd /opt
    cert-sync /etc/ssl/certs/ca-certificates.crt && \
    git clone --recursive https://github.com/zabirauf/icsharp.git /icsharp && \
    cd /icsharp/Engine && \
    mono ./.nuget/NuGet.exe restore ./ScriptCs.sln && \
    cd /icsharp && \
    mono ./.nuget/NuGet.exe restore ./iCSharp.sln && \
    xbuild ./iCSharp.sln /property:Configuration=Release /nologo /verbosity:normal && \
    mkdir -p build/Release/bin && \
    for line in $(find ./*/bin/Release/*); do cp $line ./build/Release/bin; done

# Install kernel
RUN echo '{' > /icsharp/kernel-spec/kernel.json && \
    echo '    "argv": ["mono", "/opt/icsharp/build/Release/bin/iCSharp.Kernel.exe", "{connection_file}"],' >> /icsharp/kernel-spec/kernel.json && \
    echo '    "display_name": "C#",' >> /icsharp/kernel-spec/kernel.json && \
    echo '    "language": "csharp"' >> /icsharp/kernel-spec/kernel.json && \
    echo '}' >> /icsharp/kernel-spec/kernel.json && \
    mkdir -p /opt/conda/share/jupyter/kernels/csharp
    mv /opt/icsharp/kernel-spec/kernel.json /opt/conda/share/jupyter/kernels/csharp

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER
RUN cd /opt/icsharp && \
    jupyter-kernelspec install --user kernel-spec    