## Anaconda Cheatsheet


## What is anaconda?
Anaconda is a python package manager and it is really amazing, it is popular because it brings many of the tools used in data science and machine learning with just one install.

> In a previous article I talked about how to create a virtual environment using anaconda, you can check it out [here](https://milindsoorya.site/blog/how-to-use-virtual-environment-with-conda). 🐍

In this article, I will list out some of the important anaconda commands. Let's jump right in.

> Here on out I will use conda to refer to anaconda

## Check if conda is installed and in your PATH if not install from [here](https://www.anaconda.com/products/individual-d)
```python
conda -V
```

## Check if conda is up to date
```python
conda update conda
```

## Search for python versions in conda
```python
conda search "^python$"
```
## Use conda to create a barebone virtual environment
```python
conda create -n yourenvname python
```

## Use conda to create a virtual environment containing most of the popular data science tools
```python
conda create -n yourenvname python=x.x anaconda
```

## Activate your virtual environment in conda
```python
# if you are using a UNIX operating system like ubuntu
source activate *yourenvname*

# if you are using windows
conda activate yourenvname
```

## Install additional Python packages to a conda virtual environment. 
Failure to specify “-n yourenvname” will install the package to the root Python installation.
```python
conda install -n yourenvname [package]
```

## Deactivate your conda virtual environment.
```python
conda deactivate
```

## List all conda virtual environments
```python
conda env list
```

## Delete a no longer needed conda virtual environment
```python
conda remove -n yourenvname -all
```

## Find where your conda virtual environment is stored
Before running the below code, make sure you activated your virtual environment 
```python
# on MacOS/Linux:
echo $CONDA_PREFIX

# on Windows:
echo %CONDA_PREFIX%
```

## To find path of all conda environments
```python
conda info --envs
```

That's it, thanks for reading, if you have some code to share please share in the comment section I will add it to the above list.

Don't forget  to bookmark this page for future reference 🐱‍👤


