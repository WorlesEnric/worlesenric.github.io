---
layout: post
usehighlight: true
tags: [linux, installation]
title: DL Development Environment 2 > Tools You May Need
---

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Source Mirrors ](#source-mirrors-)
    - [Ubuntu Sources (22.04 LTS jammy)](#ubuntu-sources-2204-lts-jammy)
    - [PyPI](#pypi)
    - [Docker CE](#docker-ce)
    - [Dockerhub Mirror](#dockerhub-mirror)
  - [Proxy ](#proxy-)
  - [Visual Studio Code ](#visual-studio-code-)

## Source Mirrors <a name="mirror"></a>

For users who experience the low connection qualities on certain software sources, you may consider using the source mirrors. Here we record the operations using Tsinghua Open Source Mirror (TUNA).

### Ubuntu Sources (22.04 LTS jammy)

Replace the content of `/etc/apt/sources.list` with:

```shell
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

# deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# # deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```

### PyPI

For the temporary usage, you may directly use:
`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package`

Set as default, you may use:

```shell
python -m pip install --upgrade pip
# use the below if you cannot upgrade
# python -m pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

### Docker CE

You can automatically install docker via:

```shell
export DOWNLOAD_URL="http://mirrors.tuna.tsinghua.edu.cn/docker-ce"
# For curl
curl -fsSL https://get.docker.com/ | sh
# for wget
wget -O- https://get.docker.com/ | sh
```

OR, you can install mannually. The following commands are for Ubuntu, you can change them according to the instructions [here](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/). Uninstall the old version (if there is):

```shell
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do apt-get remove $pkg; done
```

Then install the dependencies:

```shell
apt-get update
apt-get install ca-certificates curl gnupg
```

Add the repo:

```shell
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] http://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install the docker:

```shell
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Dockerhub Mirror

You can use Aliyun container mirror service to accelerate the `docker pull xxx` from docker hub.

## Proxy <a name="proxy"></a>

For the similar reasons, you may want to equip your environment with a proxy. This blog shows the utilization of [clash for linux](https://github.com/wnlen/clash-for-linux) for this purpose. You may find other solutions via google.

Download the clash via:

```shell
$ git clone https://github.com/wanhebin/clash-for-linux.git
```

Enter the directory `cd clash-for-linux` and edit `CLASH_URL` by `vim .env`.

Start the service via:

```shell
$ sudo bash start.sh
# In a new terminal
$ source /etc/profile.d/clash.sh
$ proxy_on
```

You may check if the service is correctly started:

```shell
$ netstat -tln | grep -E '9090|789.'
tcp        0      0 127.0.0.1:9090          0.0.0.0:*               LISTEN     
tcp6       0      0 :::7890                 :::*                    LISTEN     
tcp6       0      0 :::7891                 :::*                    LISTEN     
tcp6       0      0 :::7892                 :::*   

$ env | grep -E 'http_proxy|https_proxy'
http_proxy=http://127.0.0.1:7890
https_proxy=http://127.0.0.1:7890                 
```
If you wang to shutdown or restart (maybe for updating configurations):

```shell
#shuting down
sudo bash shutdown.sh
proxy_off

#restarting
sudo bash restart.sh
```

clash dashboard is by default hosted on `http://192.168.0.1:9090/`.

## Visual Studio Code <a name="vscode"></a>

Download the VSCode client [here](https://code.visualstudio.com/), then install via `sudo apt install ./<file>.deb`.