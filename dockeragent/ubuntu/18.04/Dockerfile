FROM ubuntu:18.04

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive

# # Docker-in-Docker (ref https://github.com/docker-library/docker/blob/91bbc4f7b06c06020d811dafb2266bcd7cf6c06d/18.09/Dockerfile)
# ARG DOCKER_CHANNEL="stable"
# ARG DOCKER_VERSION="19.03.9"
# ARG DOCKER_ARCH="x86_64"

RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes \
# Base components & node.js(+npm)
        && apt-get update && apt-get install -y --no-install-recommends \
                ca-certificates \
                curl \
                jq \
                git \
                iputils-ping \
                libcurl4 \
                libicu60 \
                libunwind8 \
                netcat \
        # && curl -sL https://deb.nodesource.com/setup_12.x | bash - \
        # && apt-get install nodejs \
        # && echo "Node.js & npm version" \
        # && node -v && npm -v \
# set up nsswitch.conf for Go's "netgo" implementation (which Docker explicitly uses)
# - https://github.com/docker/docker-ce/blob/v17.09.0-ce/components/engine/hack/make.sh#L149
# - https://github.com/golang/go/blob/go1.9.1/src/net/conf.go#L194-L275
# - docker run --rm debian:stretch grep '^hosts:' /etc/nsswitch.conf
        # && if [ ! -e /etc/nsswitch.conf ]; then echo 'hosts: files dns' > /etc/nsswitch.conf; fi \
        # && if ! curl -sL "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/${DOCKER_ARCH}/docker-${DOCKER_VERSION}.tgz" > docker.tgz; then \
	# 	echo >&2 "error: failed to download 'docker-${DOCKER_VERSION}' from '${DOCKER_CHANNEL}' for '${DOCKER_ARCH}'"; \
	# 	exit 1; \
	# fi \
	# && tar --extract --file docker.tgz --strip-components 1 --directory /usr/local/bin/ \
	# && rm docker.tgz \
        # && echo "Docker version" \
	# && docker --version \
        && apt-get autoremove -y

WORKDIR /azp

COPY ./start.sh .
RUN chmod +x start.sh

CMD ["./start.sh"]
