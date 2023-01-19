FROM almalinux:minimal

LABEL Name=Miniconda3_Alma Version=0.0.1

RUN microdnf update && \
    microdnf install -y --setopt=install_weak_deps=0 \
        findutils \
        git \
        libXi \
        libXtst \
        libXrandr \
        libXcursor \
        alsa-lib \
        mesa-libEGL \
        libXcomposite \
        libXScrnSaver \
        libXdamage \
        mesa-libGL \
        vi \
        wget \
        zsh \
    && microdnf clean all \
    && rm -rf /var/lib/dnf/lists/*

ENV PATH /opt/conda/bin:$PATH

CMD [ "/bin/zsh" ]
#CMD [ "/bin/bash" ]

# Arg for Docker build cache
ARG CONDA_VERSION=py310_22.11.1-1

RUN set -x && \
    UNAME_M="$(uname -m)" && \
    if [ "${UNAME_M}" = "x86_64" ]; then \
        MINICONDA_URL="https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh"; \
        SHA256SUM="00938c3534750a0e4069499baf8f4e6dc1c2e471c86a59caa0dd03f4a9269db6"; \
    elif [ "${UNAME_M}" = "aarch64" ]; then \
        MINICONDA_URL="https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-aarch64.sh"; \
        SHA256SUM="48a96df9ff56f7421b6dd7f9f71d548023847ba918c3826059918c08326c2017"; \
    fi && \
    wget "${MINICONDA_URL}" -O miniconda.sh -q && \
    echo "${SHA256SUM} miniconda.sh" > shasum && \
    if [ "${CONDA_VERSION}" != "latest" ]; then sha256sum --check --status shasum; fi && \
    mkdir -p /opt && \
    bash miniconda.sh -b -p /opt/conda && \
    rm miniconda.sh shasum && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.zshrc && \
    echo "conda activate base" >> ~/.zshrc && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/conda clean -afy

