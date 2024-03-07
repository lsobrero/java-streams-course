FROM ubuntu:22.04

ARG USERNAME=vscode
ARG USER_GROUP_NAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

ARG PKG="git vim curl unzip zip sudo build-essential libz-dev zlib1g-dev"

SHELL ["/bin/bash", "-c"]


RUN apt-get update \
    && apt-get install -y ${PKG} \
    && groupadd --gid ${USER_GID} ${USER_GROUP_NAME} \
    && useradd --uid ${USER_UID} --shell /bin/bash --gid ${USER_GID} -m ${USERNAME} \
    && echo %${USER_GROUP_NAME} ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/${USER_GROUP_NAME} \
    && chmod 0440 /etc/sudoers.d/${USER_GROUP_NAME}


ARG JAVA_VERSION=17.0.10-amzn
ARG MAVEN_VERSION=3.8.8

USER ${USERNAME}

RUN curl -s "https://get.sdkman.io" | bash \
    && echo sdkman_curl_connect_timeout=50 >> ~/.sdkman/etc/config \
    && echo sdkman_curl_max_time=500 >> ~/.sdkman/etc/config

# need to improve using ARG's
RUN source "$HOME/.sdkman/bin/sdkman-init.sh" \
    && sdk install java 17.0.10-amzn \
    && sdk install maven 3.9.6

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
    && source "$HOME/.nvm/nvm.sh" \
    && nvm install v18.19.1


RUN echo 'shopt -s histappend' >> /home/vscode/.bashrc && \
    echo 'PROMPT_COMMAND=history -a;$PROMPT_COMMAND' >> /home/vscode/.bashrc && \
    sed -i 's/OSH_THEME="font"/OSH_THEME="powerline-plain"/g' /home/vscode/.bashrc && \
    echo 'HISTFILE=/home/vscode/.bash_history' >> /home/vscode/.bashrc \
    echo 'alias ll=ls -al' >> /home/vscode/.bashrc

ENTRYPOINT ["/bin/bash"]