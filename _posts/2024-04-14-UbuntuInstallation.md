---
layout: post
usehighlight: true
tags: [linux, installation]
title: DL Development Environment 1 > System Installation and Configuration
---

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [OS Installation ](#os-installation-)
    - [Preparing Ubuntu Image ](#preparing-ubuntu-image-)
  - [OS Configuration](#os-configuration)
    - [GPU Related Configuration ](#gpu-related-configuration-)
  
## OS Installation <a name="osinstallation"></a>

You need:

* A laptop or PC with at least 25GB of storage space.

* A flash drive (12GB or above recommended).

### Preparing Ubuntu Image <a name="preparation"></a>

Download the image from [here](https://ubuntu.com/download/desktop). We use the Ubuntu 22.04 LTS for this guide.

Staying consistent to the official tutorial, we use [balenaEtcher](https://etcher.balena.io/) to create the bootable USB stick. Do the following on balenaEtcher:

* Select your downloaded ISO
* Choose your USB flash drive
* Click Flash
  
You may need to modify the boot option, for this, you may hold `F12` while the computer starts.

Setting up Ubuntu installation:

* Computer name: Name-PC
* Name: Name
* User name: name
* Password: 
  
## OS Configuration

Ref: [Ubuntu 22.04 for DL](https://gist.github.com/amir-saniyan/b3d8e06145a8569c0d0e030af6d60bea)

You may update Ubuntu before further operations:

```shell
$ sudo apt update
$ sudo apt full-upgrade --yes
$ sudo apt autoremove --yes
$ sudo apt autoclean --yes
$ reboot
```

You can create a full-update script `~/full-update.sh` to pack these operations:

```shell
#!/usr/bin/env bash

if [ "$EUID" -ne 0 ]
  then echo "Error: Please run as root."
  exit
fi

clear

echo "################################################################################"
echo "Updating list of available packages..."
echo "--------------------------------------------------------------------------------"
apt update
echo "################################################################################"
echo

echo "################################################################################"
echo "Upgrading the system by removing/installing/upgrading packages..."
echo "--------------------------------------------------------------------------------"
apt full-upgrade --yes
echo "################################################################################"
echo

echo "################################################################################"
echo "Removing automatically all unused packages..."
echo "--------------------------------------------------------------------------------"
apt autoremove --yes
echo "################################################################################"
echo

echo "################################################################################"
echo "Clearing out the local repository of retrieved package files..."
echo "--------------------------------------------------------------------------------"
apt autoclean --yes
echo "################################################################################"
echo
```

Install the Chrome web browser by (get the deb file [here](https://www.google.com/chrome/)):

```shell
$ sudo dpkg -i google-chrome-stable_current_amd64.deb
```

Install the developement tools by:

```shell
$ sudo apt install build-essential pkg-config cmake cmake-qt-gui ninja-build valgrind
```

Install Python3 and venv by:

```shell
$ sudo apt install python3 python3-wheel python3-pip python3-venv python3-dev python3-setuptools
```

Install Git by:

```shell
$ sudo apt install git
$ git config --global user.name "Name"
$ git config --global user.email "name@domain.com"
$ git config --global core.editor "gedit -s"
```
### GPU Related Configuration <a name="GPU"></a>
Now it comes to the steps for setting up NVIDIA toolkits. The process here may not align with your situation, check the NVIDIA official toturial whenever there are mistakes.

Check the display hardware by:

```shell
$ sudo lshw -C display
```

**Check CUDA and NVIDIA Driver Compatibilities** [here](https://docs.nvidia.com/deeplearning/cudnn/reference/support-matrix.html).

**Check TensorFlow and CUDA Compatibilities** [here](https://www.tensorflow.org/install/gpu) and [here](https://www.tensorflow.org/install/source#gpu).

**Check Torch and CUDA Campatibilities** [here](https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix).

Here we use CUDA 11.8, install the NVIDIA Driver by:

```shell
$ sudo apt install nvidia-driver-535
```

Install the prerequisites:

```shell
$ sudo apt install linux-headers-$(uname -r)
```

Download CUDA 11.8 via:

```shell
$ wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
```

Install CUDA 11.8 by (select without driver):

```shell
$ sudo ./cuda_11.8.0_520.61.05_linux.run --override
```

Setting up the environment variables:

```shell
export PATH=$PATH:/usr/local/cuda-11.8/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.8/lib64:/usr/local/cuda-11.8/extras/CUPTI/lib64
```

Test by:

```shell
$ nvidia-smi
$ NVCC -v
```

Install cuDNN v8.6 for CUDA 11.8. Login and download the deb file [here](https://developer.nvidia.com/compute/cudnn/secure/8.6.0/local_installers/11.8/cudnn-local-repo-ubuntu2204-8.6.0.163_1.0-1_amd64.deb)

Then install by:

```shell
$ sudo dpkg -i cudnn-local-repo-ubuntu2204-8.6.0.163_1.0-1_amd64.deb
$ sudo cp /var/cudnn-local-repo-ubuntu2204-8.6.0.163/cudnn-local-FAED14DD-keyring.gpg /usr/share/keyrings/
$ sudo apt update
$ sudo apt install libcudnn8
$ sudo apt install libcudnn8-dev
$ sudo apt install libcudnn8-samples
```

Then reboot by `sudo reboot`
