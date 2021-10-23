---
title: "Quick Python Setup"
date: 2019-07-02
draft: false
categories:
- Code
tags:
- Python
- Environments
---

## Motivation
Python has increased in popularity to near ubiquity in the past five years. While the Python community (correctly) professes 
simplicity as a major accomplishment of the language, I still get a lot of questions about how to get a python environment 
setup properly. There are some lengthy guides out there on this - this post will aim to summarize and explain the relevant 
components to getting started.

**Note: skip to bottom if you want quick install commands**

### The Pieces
- **_Python_** is a language. Combination of syntax rules, semantics, keyword commands, an interpreter that can execute code abiding the rules. 
- **_package_** is a self-contained, reusable piece of python code. Often a directory containing one or more python files (or more subdirectories). One typically packages python code in order to share it. There are some packages that come with the language itself (called "builtin") and others that users can install optionally (called "3rd party" packages)
- **_environment_** is a combination of a Python installation and a collection of packages. Pip can only install python packages - not any precompiled binaries (conda can do that!)
- **_conda_** is a 3rd party package manager, and frankly I find it superior to pip in almost every way. It can install packages in a variety of languages, precompiled binaries (meaning numpy will install _much_ faster).
- **_miniconda_** is an environment that contains bare-bones packages only, including the conda package manager.
- **_Anaconda_** is an environment that contains a _large_ amount of packages, aimed at scientific computing (such as numpy, scipy, etc)

### The Process
Recall that this guide is designed for novice-users, or as a reference for other users. I will assume that the user is 
able to use a desktop GUI (not headless).

#### Using Pip (not recommended)
Install python using the [os-specific Python installation programs](https://www.python.org/downloads/). I recommend 
installing the latest version of Python 3.*, since most new packages do not include support for older versions of 2.7. 
The program will install all the base packages you require, including pip. At which point you can open up a command line 
(terminal on Mac or CMD on Win), and use commands like `pip install x` to install a package named "x" (for more pip commands 
see the [docs](https://pip.pypa.io/en/stable/quickstart/)).

#### Using Conda (recommended)
Install **miniconda** using the [install program](https://docs.conda.io/en/latest/miniconda.html). Once installed, create 
your Python environment by doing the following:

1. `conda create --name env1 python=3.7` (you can replace "env1" with the preferred name of the environment, probably something related to your current coding project. You can also replace "3.7" with whatever version of python you want to use)
2. This will do some thinking then print a list of packages that will be installed, accept this by typing `Y`
3. Your environment is created! You can activate this environment (on Mac/Linux by using `source activate env1` or on Win by using `activate env1`). 
4. Now that you are "in" the environment you created, you can install other packages into the environment using `conda install x` to install package "x"
5. You can also install other packages from a text file using `conda install --file requirements.txt`, where "requirements.txt" is the requirements file containing a list of package names (one on each line).
<br><br>

There are, of course, more complicated options available using conda, for those see 
the [conda cheat sheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)

## TL;DR - Get Python Setup
The below steps give a quick, general-purpose environment setup.

1. Install [miniconda](https://docs.conda.io/en/latest/miniconda.html)
2. `conda create --name env1 python=3.7`
3. `source activate env1`
4. `conda install numpy scipy pandas`
<br><br>

The "env1" directory will be installed in the user x directory (`~/.anaconda` on Mac or `C:\Users\x\.anaconda` on Win). 
If you are using a python editor (like PyCharm) that wants to be "pointed" to the environment, then you will configure 
it to look at the "env1" directory under the ".anaconda" folder.

## Conda env shortcut
It is also possible to create a clickable shortcut to the conda environment, see [the docs](https://anaconda.org/anaconda/console_shortcut) for more.
