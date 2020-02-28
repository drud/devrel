FROM jenkins/jenkins:lts
ARG DEBIAN_FRONTEND=noninteractive

USER root

RUN apt-get update \
	&& apt-get install -y --no-install-recommends build-essential ca-certificates curl file g++ git locales make uuid-runtime jq \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
	&& sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
	&& dpkg-reconfigure locales \
	&& update-locale LANG=en_US.UTF-8

USER jenkins
WORKDIR /usr/share/jenkins/ref
ENV LANG=en_US.UTF-8 \
	PATH=/usr/share/jenkins/ref/.linuxbrew/bin:/usr/share/jenkins/ref/.linuxbrew/sbin:$PATH \
	SHELL=/bin/bash

RUN git clone https://github.com/Homebrew/brew /usr/share/jenkins/ref/.linuxbrew/Homebrew \
	&& mkdir /usr/share/jenkins/ref/.linuxbrew/bin \
	&& ln -s ../Homebrew/bin/brew /usr/share/jenkins/ref/.linuxbrew/bin/ \
	&& echo 'eval $(/usr/share/jenkins/ref/.linuxbrew/bin/brew shellenv)' >>~/.profile \
	&& brew config \
	&& brew tap unn/homebrew-ddev-live \
	&& brew install ddev-live

RUN git clone https://github.com/drud/devrel.git /usr/share/jenkins/ref/devrel