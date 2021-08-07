## How To Set Up Jupyter Notebook with Python 3 on Ubuntu 20.04

## Introduction

An open-source web application, [Jupyter Notebook](http://jupyter.org/) lets you create and share interactive code, visualisations, and more. It is an essential software used by data scientists. It is often used for working with data, statistical modelling, and machine learning.

If you are using windows the best way to setup Jupyter Notebook is by using [Anaconda](https://www.anaconda.com/products/individual).

By the end of this guide, you will be able to run Python 3 code using Jupyter Notebook running on your system.

## Prerequisites

In order to complete this guide, you should have a Ubuntu system with non-root user with sudo privileges configured.

## Step 1 — Set Up Python

update the local `apt` package index and then download and install the packages:

```bash
sudo apt update
```

Next, install pip and the Python header files, which are used by some of Jupyter’s dependencies:

```bash
sudo apt install python3-pip python3-dev
```

## Step 2 — Create a Python Virtual Environment for Jupyter

Upgrade pip and install the package by typing:

```bash
sudo -H pip3 install --upgrade pip
sudo -H pip3 install virtualenv
```

The `-H` flag ensures that the security policy sets the `home` environment variable to the home directory of the target user.

With `virtualenv` installed, we can start forming our environment. Create and move into a directory where we can keep our project files. 

```bash
mkdir ~/ml_projects
cd ~/ml_projects
```

Within the project directory, we’ll create a Python virtual environment. 

```bash
user@pc:~/ml_projects$ virtualenv houseprices_env
```

it will install a local version of Python and a local version of pip. We can use this to install and configure an isolated Python environment for Jupyter.

Before we install Jupyter, we need to activate the virtual environment. You can do that by typing:

```bash
source houseprices_env/bin/activate
```

## Step 3 — Install Jupyter

> Once the virtual environment is activated, use `pip` instead of `pip3`, even if you are using Python 3. The virtual environment’s copy of the tool is always named `pip`, regardless of the Python version.

```bash
(houseprices_env) user@pc:~/ml_projects$ pip install jupyter
```

## Step 4 — Run Jupyter Notebook

You now have everything you need to run Jupyter Notebook! To run it, execute the following command:

```bash
(houseprices_env) user@pc:~/ml_projects$ jupyter notebook
```

Now the notebook should open automatically in your browser. If it is not opening you can manually copy the URL from the terminal it will usually have a port number of 8888.