## Installing TensorFlow on M1 MacBook Air with GPU (Metal)


You can now leverage Apple’s tensorflow-metal PluggableDevice in TensorFlow v2.5 for accelerated training on Mac GPUs directly with Metal. Learn more [here](https://developer.apple.com/metal/tensorflow-plugin/).

### OS Requirements

    macOS 12.0+ (latest beta)

### Currently Not Supported

    Multi-GPU support
    Acceleration for Intel GPUs
    V1 TensorFlow Networks

## Table of contents

1. [What is tensorflow?](#What-is-tensorflow)
2. [What is miniforge?](#what-is-miniforge)
3. [What is miniconda?](#what-is-miniconda)
4. [Uninstalling existing anaconda/conda from macOS](#uninstalling)
   1. Install the Anaconda-Clean package
   2. Remove all Anaconda-related files
   3. Remove entire anaconda installation directory
5. [Step 1: Install Miniforge](#install-miniforge)
   1. Download and install Conda env
   2. Create an anaconda environment
   3. Activate the environment
6. [Step 2: Install TensorFlow](#install-tensorflow)
   1. Install TensorFlow dependencies
   2. Install base TensorFlow
   3. Install tensorflow-metal plugin
   4. Install common Data Science packages
7. [References](#references)
8. [Related Articles](#related)

<a name="What-is-tensorflow"></a>
## What is tensorflow?

TensorFlow is open source deep learning framework created by developers at Google and released in 2015. It is actively used at Google both for research and production needs.

Please check references if you would like to read more about TensorFlow and other popular deep learning libraries.

<a name="what-is-miniforge"></a>
## What is miniforge?

Miniforge is the community (conda-forge) driven
minimalistic conda installer. Subsequent package installations come thus from
conda-forge channel.

<a name="what-is-miniconda"></a>
## What is miniconda?

Miniconda is the Anaconda (company) driven minimalistic
conda installer. Subsequent package installations come from the anaconda
channels (default or otherwise).

Note: Uninstall Anaconda/Anaconda Navigator and other related previously installed version of conda-based installations. Anaconda and Miniforge cannot co-exist together.

<a name="uninstalling"></a>
## Uninstalling existing anaconda/conda from macOS

### Install the Anaconda-Clean package from Anaconda Prompt

```bash
conda install anaconda-clean
```

### Remove all Anaconda-related files and directories without being prompted to delete each one

```bash
anaconda-clean --yes
```

### Remove entire anaconda installation directory from macOS

```bash
which anaconda
```

After running above command you should see the directory where anaconda is installed.

now recursively remove that folder

```bash
rm -rf YOUR-ANACONDA-DIRECTORY
```

<a name="install-miniforge"></a>
## Step 1: Install Miniforge

> Please note that this tutorial is for arm64 : Apple Silicon

### Download and install Conda env

```bash
chmod +x ~/Downloads/Miniforge3-MacOSX-arm64.sh
sh ~/Downloads/Miniforge3-MacOSX-arm64.sh
source ~/miniforge3/bin/activate
```

### Create an anaconda environment

```bash
conda create -n tf python=3.8
```

### Activate the environment

```bash
conda activate tf
```

<a name="install-tensorflow"></a>
## Step 2: Install TensorFlow

### Install TensorFlow dependencies

```bash
conda install -c apple tensorflow-deps
```

### Install base TensorFlow

```bash
python -m pip install tensorflow-macos
```

### Install tensorflow-metal plugin

```bash
python -m pip install tensorflow-metal
```

### Install common Data Science packages

```bash
pip install tensorflow-datasets pandas jupyterlab
```

<a name="references"></a>
## References

- [Getting Started with tensorflow-metal PluggableDevice](https://developer.apple.com/metal/tensorflow-plugin/)
- [What is the difference between miniconda and miniforge?](https://stackoverflow.com/questions/60532678/what-is-the-difference-between-miniconda-and-miniforge)
- [PyTorch vs. TensorFlow: Which Framework Is Best for Your Deep Learning Project?](https://builtin.com/data-science/pytorch-vs-tensorflow)

<a name="related"></a>
## Related Articles

- [Run Ubuntu on M1 Macbook Air using UTM](https://www.milindsoorya.com/blog/run-ubuntu-on-m1-macbook-air-using-utm)
- [Add anaconda to right-click menu in windows](https://www.milindsoorya.com/blog/add-anaconda-to-right-click-menu-in-windows)
- [How to open sublime text from the windows command line](https://www.milindsoorya.com/blog/how-to-open-sublime-text-from-the-windows-command-line)
