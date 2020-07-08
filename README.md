# Introduction

This repo provides a `Dockerfile` and guidance on how to build a [self-host Azure Pipelines agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux) and deploy it as container on a container service such as Docker or Kubernetes cluster.

## Customize, build and publish the docker image

This repository provides a [Dockerfile](dockeragent/ubuntu/18.04/Dockerfile) containing basic tools needed to run a [self-hosted Azure Pipelines agent on Linux](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux).
You are free to customize it with additional tools you need for your project (e.g. JDK). Then build and publish the image to a docker registry if your choice.

Run the following commands to build and publish your image to [Docker Hub](https://hub.docker.com/):

```shell
cd dockeragent/ubuntu/18.04
docker build -t <yourDockerHubUsername>/myazpagent:ubuntu-18.04 -f Dockerfile .
docker login
docker push <yourDockerHubUsername>/myazpagent:ubuntu-18.04
```

Or you can use the [pre-built image](https://hub.docker.com/r/btnguyen2k/azpagent-base).
