FROM nvidia/cuda:12.4.1-base-ubuntu22.04

# Configure image
ARG PYTHON_VERSION=3.10
ARG DEBIAN_FRONTEND=noninteractive

# Install apt dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential cmake \
    git git-lfs openssh-client \
    nano vim less util-linux \
    htop atop nvtop \
    sed gawk grep curl wget \
    tcpdump sysstat screen tmux \
    libglib2.0-0 libgl1-mesa-glx libegl1-mesa ffmpeg \
    python${PYTHON_VERSION} python${PYTHON_VERSION}-venv \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install gh cli tool
RUN (type -p wget >/dev/null || (apt update && apt-get install wget -y)) \
    && mkdir -p -m 755 /etc/apt/keyrings \
    && wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg | tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
    && chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && apt update \
    && apt install gh -y \
    && apt clean && rm -rf /var/lib/apt/lists/*

# Setup `python`
RUN ln -s /usr/bin/python3 /usr/bin/python

# Install poetry
RUN curl -sSL https://install.python-poetry.org | python -
ENV PATH="/root/.local/bin:$PATH"
RUN echo 'if [ "$HOME" != "/root" ]; then ln -sf /root/.local/bin/poetry $HOME/.local/bin/poetry; fi' >> /root/.bashrc
RUN poetry config virtualenvs.create false
RUN poetry config virtualenvs.in-project true

# Set EGL as the rendering backend for MuJoCo
ENV MUJOCO_GL="egl"
