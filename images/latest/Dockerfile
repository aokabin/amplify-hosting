FROM amazonlinux:2

# https://docs.docker.com/engine/reference/builder/#maintainer-deprecated
LABEL maintainer="Amazon AWS"

# Framework Versions
ENV VERSION_NODE_16=16
ENV VERSION_NODE_DEFAULT=$VERSION_NODE_16

ENV VERSION_YARN=1.22.17
ENV VERSION_AMPLIFY=7.6.26
ENV VERSION_SM=0.2.12-rc.1
ENV VERSION_GRUNT_CLI=1.4.3
ENV VERSION_BOWER=1.8.13
ENV VERSION_VUEPRESS=1.9.7
ENV VERSION_GATSBY_CLI=4.6.0
ENV VERSION_CYPRESS=9.4.1

# UTF-8 Environment
ENV LANGUAGE en_US:en
ENV LANG=en_US.UTF-8
ENV LC_ALL en_US.UTF-8

## Install OS packages
RUN touch ~/.bashrc
RUN yum -y update && \
    yum -y install \
        alsa-lib-devel \
        autoconf \
        automake \
        bzip2 \
        bison \
        bzr \
        cmake \
        expect \
        fontconfig \
        git \
        gcc-c++ \
        GConf2-devel \
        gtk2-devel \
        gtk3-devel \
        libnotify-devel \
        libpng \
        libpng-devel \
        libffi-devel \
        libtool \
        libX11 \
        libXext \
        libxml2 \
        libxml2-devel \
        libXScrnSaver \
        libxslt \
        libxslt-devel \
        libyaml \
        libyaml-devel \
        make \
        nss-devel \
        openssl-devel \
        openssh-clients \
        patch \
        procps \
        python3 \
        python3-devel \
        readline-devel \
        sqlite-devel \
        tar \
        tree \
        unzip \
        wget \
        which \
        xorg-x11-server-Xvfb \
        zip \
        zlib \
        zlib-devel \
    yum clean all && \
    rm -rf /var/cache/yum

## Install Node
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
RUN /bin/bash -c ". ~/.nvm/nvm.sh && \
    nvm install $VERSION_NODE_16 && nvm use $VERSION_NODE_16 && chown -R root:root /root/.nvm &&  \
    npm install -g yarn@${VERSION_YARN} sm@${VERSION_SM} grunt-cli@${VERSION_GRUNT_CLI} bower@${VERSION_BOWER} vuepress@${VERSION_VUEPRESS} && \
    npm install -g --unsafe-perm=true gatsby-cli@${VERSION_GATSBY_CLI} && \
    nvm alias default ${VERSION_NODE_DEFAULT} && nvm cache clear"

# Handle yarn for any `nvm install` in the future
RUN echo "yarn@${VERSION_YARN}" > /root/.nvm/default-packages

## Install awscli
RUN /bin/bash -c "pip3.7 install awscli && rm -rf /var/cache/apk/*"
# RUN /bin/bash -c "pip3.8 install awscli && rm -rf /var/cache/apk/*"
# RUN /bin/bash -c "pip3.9 install awscli && rm -rf /var/cache/apk/*"

## Install SAM CLI
RUN /bin/bash -c "pip3.7 install aws-sam-cli"
# RUN /bin/bash -c "pip3.8 install aws-sam-cli"
# RUN /bin/bash -c "pip3.9 install aws-sam-cli"

## Installing Cypress
RUN /bin/bash -c ". ~/.nvm/nvm.sh && \
    nvm use ${VERSION_NODE_DEFAULT} && \
    npm install -g --unsafe-perm=true --allow-root cypress@${VERSION_CYPRESS}"

## Install AWS Amplify CLI for all node versions
RUN /bin/bash -c ". ~/.nvm/nvm.sh && nvm use ${VERSION_NODE_16} && \
    npm config set user 0 && npm config set unsafe-perm true && \
    npm install -g @aws-amplify/cli@${VERSION_AMPLIFY}"

# # Install dotnet sdk and host 3.1
# RUN rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
# RUN yum -y install dotnet-host-3.1.23
# RUN yum -y install dotnet-sdk-3.1

# ## Install amazon dotnet tools
# RUN dotnet tool install -g Amazon.Lambda.Tools
# RUN dotnet tool install -g Amazon.Lambda.TestTool-3.1

## Environment Setup
RUN echo export PATH="\
/root/.nvm/versions/node/${VERSION_NODE_DEFAULT}/bin:\
$PATH" >> ~/.bashrc && \
     echo "nvm use ${VERSION_NODE_DEFAULT} 1> /dev/null" >> ~/.bashrc

ENTRYPOINT [ "bash", "-c" ]
