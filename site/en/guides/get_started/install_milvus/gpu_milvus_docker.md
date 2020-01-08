---
id: gpu_milvus_docker.md
title: Install GPU-enabled Milvus
sidebar_label: Install GPU-enabled Milvus
---

# Install GPU-enabled Milvus

## Prerequisites

#### System requirements

| Operating system | Supported versions |
| :--------------- | :----------------- |
| CentOS           | 7.5 or higher      |
| Ubuntu LTS       | 18.04 or higher    |

#### Hardware requirements

| Component  | Recommended configuration             |
| ---------- | ------------------------------------- |
| CPU        | Intel CPU Haswell or higher           |
| GPU        | NVIDIA Pascal or higher               |
| RAM        | 8 GB or more (depends on data volume) |
| Hard drive | SATA 3.0 SSD or higher                |

#### Milvus Docker requirements

- [Install Docker](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/) 19.03 or higher on your local host machine.
- Install NVIDIA driver 418 or higher.
- [Install NVIDIA Docker support](https://github.com/NVIDIA/nvidia-docker)

## Step 1 Confirm Docker status

Confirm that the Docker daemon is running in the background:

```shell
$ docker info
```

If you do not see the server listed, start the **Docker** daemon.

> Note: On Linux, Docker needs sudo privileges. To run Docker command without `sudo`, create the `docker` group and add your user. For details, see the [post-installation steps for Linux](https://docs.docker.com/install/linux/linux-postinstall/).

## Step 2 Pull Milvus image

Pull the GPU-enabled image:

```shell
$ docker pull milvusdb/milvus:0.6.0-gpu-d120719-2b40dd
```

## Step 3 Download configuration files

```shell
# Create Milvus file
$ mkdir -p /home/$USER/milvus/conf
$ cd /home/$USER/milvus/conf
$ wget https://raw.githubusercontent.com/milvus-io/docs/blob/v0.6.0/assets/config/server_config.yaml
$ wget https://raw.githubusercontent.com/milvus-io/docs/blob/v0.6.0/assets/config/log_config.conf
```

> Note: In case you encounter problems downloading configuration files using `wget` command, you can also create the `server_config.yaml` and `log_config.conf` files under `/home/$USER/milvus/conf`, then copy and paste the content from [server config file](https://github.com/milvus-io/docs/blob/v0.6.0/assets/config/server_config.yaml) and [log config file](https://github.com/milvus-io/docs/blob/v0.6.0/assets/config/log_config.conf).

## Step 4 Start Docker container

```shell
# Start Milvus
$ docker run -d --name milvus_gpu --gpus all \
-p 19530:19530 \
-p 8080:8080 \
-v /home/$USER/milvus/db:/var/lib/milvus/db \
-v /home/$USER/milvus/conf:/var/lib/milvus/conf \
-v /home/$USER/milvus/logs:/var/lib/milvus/logs \
milvusdb/milvus:0.6.0-gpu-d120719-2b40dd
```


The `docker run` options used in the above command are defined as follows:

- `-d`: run container in background and print container ID
- `--name`: assign a name to the container
- `--gpus`: GPU devices to add to the container (‘all’ to pass all GPUs)
- `-p`: publish a container’s port(s) to the host
- `-v`: bind mount a volume

Finally confirm Milvus running status by the following command:

```shell
# Confirm Milvus status
$ docker ps
```

If Milvus server is not successfully started, you can check the error logs by the following command.

```shell
# Get id of the container running Milvus
$ docker ps -a
# Check docker logs
$ docker logs <milvus container id>
```

## What's next

- If you're just getting started with Milvus:

  - [Try an example program](../example_code.md)
  - [Learn more about Milvus operations](../../milvus_operation.md)
  - [Try Milvus Bootcamp](https://github.com/milvus-io/bootcamp)
  
- If you're ready to run Milvus in production:

  - Build a [monitoring and alerting system](../../monitor.md) to check real-time application performance
  - Tune Milvus performance through [configuration](../../../reference/milvus_config.md)
  
- If you want to run Milvus on machines without GPU:
  
  - [Install CPU-only Milvus](cpu_milvus_docker.md)