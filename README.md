# Introduction

This repo provides a `Dockerfile` and guidance on how to build a [self-host Azure Pipelines agent](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker#linux) and deploy it as container on a container service such as Docker or Kubernetes cluster.

## Prerequisites

**Azure DevOps personal access token**:
A personal access token (PAT) is used as an alternate password to authenticate into Azure DevOps. Use PAT instead of your password to register the agent with Azure DevOps Agent Pool. Create one by following [these instructions](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

> The PAT must have at least **Agent Pools (Read & manage)** scope.

**Azure Pipelines agent pool**:
The Azure Pipelines agent will join an [agent pool](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/pools-queues) so that it can be used. You can create a new agent pool or use an existing one.

## Customize your own image

This repository provides a [Dockerfile](dockeragent/ubuntu/18.04/Dockerfile) containing basic tools needed to run a [self-hosted Azure Pipelines agent on Linux](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker#linux).
You are free to customize it with additional tools you need for your project (e.g. JDK). Then build and publish the image to a docker registry if your choice.

Run the following commands to build and publish your image to [Docker Hub](https://hub.docker.com/):

```shell
cd dockeragent/ubuntu/18.04
docker build -t <yourDockerHubUsername>/myazpagent:ubuntu-18.04 -f Dockerfile .
docker login
docker push <yourDockerHubUsername>/myazpagent:ubuntu-18.04
```

Or you can use the [pre-built image](https://hub.docker.com/r/btnguyen2k/azpagent-base).

## Deploy the agent to Docker

The Azure Pipelines agent can be deployed as Docker container using the following command:

```shell
docker run -d -e AZP_URL=<your-azuredepops-instance> -e AZP_TOKEN=<pat> -e AZP_POOL=<azp-agentpool-name> -e AZP_AGENT_NAME=<agent-name> --name mycontainer myazpagent:ubuntu-18.04
```

**[Environment variables](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker#environment-variables)**

|Env variable  |Description|Default value|
|--------------|------------------------------------------------------------|-------------|
|AZP_URL       |The URL of the Azure DevOps or Azure DevOps Server instance.<br>Example `https://dev.azure.com/myaccount/`| |
|AZP_TOKEN     |[Personal access token (PAT)](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate#create-a-pat) with **Agent Pools (read, manage)** scope.| |
|AZP_AGENT_NAME|Agent name|the container hostname|
|AZP_POOL      |Agent pool name|`Default`|
|AZP_WORK      |Work directory|`_work`|

**Mount volumn to persist data in work directory**

```shell
docker run -d -e AZP_URL=... -e AZP_TOKEN=... -e AZP_POOL=... -e AZP_AGENT_NAME=... -e AZP_WORK=/workspace -v /home/workspace/azp:/workspace --name mycontainer myazpagent:ubuntu-18.04
```

## Deploy the agent to Kubernetes cluster

See [samples](k8s-samples/).
