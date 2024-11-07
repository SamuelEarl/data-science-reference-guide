# Data Science Reference Guide

This repo will contain all of my data science notes.

---

# How To Setup Data Science Projects

## Install Anaconda3

1. Go to Anaconda's website and install Anaconda for your operating system.
2. After Anaconda is installed, then make sure that Python3 and Pip3 are on your PATH variable. In a terminal run `which python3` and then `which pip3`. Those commands should return a file path to your Anaconda installation.
3. `conda` is Anaconda's package manager. Make sure that your conda installation worked: `conda --version`. If that returns a version number, then you have installed Anaconda3 and conda correctly.

<br><br>

## Run JupyterLab inside a virtual environment (RECOMMENDED)

Every data science project you do will require some combination of external libraries, sometimes with specific versions that differ from the specific versions you used for other projects. If you were to have a single Python installation, these libraries would conflict and cause you all sorts of problems. 

The standard solution is to use virtual environments, which are sandboxed Python environments that maintain their own versions of Python libraries (and, depending on how you set up the environment, of Python itself).

As a matter of good discipline, you should always work in a virtual environment, and never use the "base" Python installation. 

_Source: "Data Science from Scratch - 2nd Ed," pages 37, 24_

<br>

### Step 1: Create an `environment.yml` file in your project root directory

You can create and activate virtual environments with conda. (See [Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).)

First, create an `environment.yml` file in your project root directory. Here is an example of an `environment.yml` file:

```yml
name: name-of-virtual-environment
channels:
  - conda
  - conda-forge
dependencies:
  - python=3.12.4
  - pip
  - python-graphviz==0.20.3
  - pip:
    - scikit-learn==1.5.1
    - pandas==2.2.2
    - seaborn==0.13.2
    - imbalanced-learn==0.12.3
    - numpy==2.0.0
    - matplotlib==3.9.1
    - kmodes==0.12.2
    - six==1.16.0
    - pydotplus==2.0.2
    - statsmodels==0.14.2
    - ipykernel==6.29.5
    - jupyterlab==4.2.4
```

**What is `ipykernel`?**

*The Jupyter Notebook and other frontends automatically ensure that the IPython kernel is available. However, if you want to use a kernel with a different version of Python, or in a virtualenv or conda environment, you’ll need to install that manually.* (Source [Installing the IPython kernel](https://ipython.readthedocs.io/en/stable/install/kernel_install.html#:~:text=The%20IPython%20kernel%20is%20the,need%20to%20install%20that%20manually.))

Since we are using JupyterLab, instead of Jupyter Notebooks, we are able to run it inside of a virtual environment in a pretty automated and easy way. I don't think that is possible with Jupyter Notebooks, but I could be wrong.

NOTE: If you need to use Jupyter Notebooks (instead of JupyterLab), then I think you will have to install `ipykernel` and run Jupyter Notebooks inside a virtual environment a different (and more manual) way. For details, refer to the Preface of the book I own titled "Data Science for Marketing Analytics".

<hr>
<br>

For reference, here is an `environment.yml` file that could be used with a Python backend for web development, if you are not using Docker as your development environment:

```yml
# This file is used with conda virtual environments for development. Docker development environments use the `requirements.txt` file.

name: name-of-virtual-environment
dependencies:
  - python=3.12
  - pip=24.2
  # You need to list pip as a dependency and then list any packages that need to be installed by pip after all the dependencies that are to be installed by conda.
  # https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#using-pip-in-an-environment
  # https://stackoverflow.com/questions/61715343/solving-environment-failed-when-trying-to-set-up-python-virtual-envrironment
  - pip:
    - litestar[standard]==2.12.1
    # - uvicorn==0.32.0 # litestar[standard] includes commonly used extras like uvicorn, so it is not necessary to specify uvicorn as a separate package.
    - python-dotenv==1.0.1
    - pyjwt==2.9.0
    - python-multipart==0.0.12
    - aiofiles==24.1.0
    - edgedb==2.1.0
```

<br>
<hr>
<br>

### Step 2: Create your virtual environment

In a terminal window `cd` into your project directory and run

```
conda env create -f environment.yml
```

This will install the virtual environment that is specified in your `environment.yml` file. In case you see a prompt asking you to confirm before proceeding, type `y` and press Enter to continue creating the environment. Depending on your system configuration, it may take a while for the process to complete.

<br>

### Step 3: Verify that the new virtual environment was installed correctly

In your terminal type:

```
conda env list
```

You should see the name of your virtual environment in that list, which is the `name` specified in your `environment.yml` file.

<br>


### Step 4.a.: Use Jupyter Notebooks in VSCode

There are a few different notebook options that you can use. This shows how to use Jupyter Notebooks inside VSCode.

NOTE: When you install the Python language support extension in VSCode you should also be prompted to install the Polyglot Notebooks extension, which provides support for Jupyter Notebooks inside of VSCode.

1. Open your project folder in VSCode.
2. Create a new file with this naming convention: `<file-name>.ipynb`. VSCode will open the file inside a notebook.
3. Activate Your Virtual Environment: 
    1. In the top left corner of the notebook file you will see a button for `Select Kernel`. (NOTE: That button looks more like a label than a button.)
    2. Click that button >> `Python Environments...` >> Select the virtual environment that you created previously. 
    3. You should now see that `Select Kernel` has been replaced by the virtual environment that you selected.

<br>

### Step 4.b. (Alternative): Use JupyterLab inside your virtual environment

There are a few different notebook options that you can use. This shows how to use JupyterLab inside your virtual environment.

#### Step 4.b.i: Activate your virtual environment

```
conda activate <name-of-virtual-environment>
```

Once your virtual environment has been activated, your command prompt should be prefixed with the name of your virtual environment. For example:

```
(name-of-virtual-environment) ~/path/to/data/science/project(name-of-git-branch)$
```

<br>

#### Step 4.b.ii.: Run JupyterLab inside your virtual environment

Run the following command inside your activated virtual environment:

```
jupyter-lab
```

When you run `jupyter-lab` the JupyterLab server will run and an instance of JupyterLab will open up in a browser window. The following are a couple of indicators that you are running JupyterLab from inside a virtual environment:

1. The output in the terminal where you ran `jupyter-lab` should have the following two lines *(which indicate that JupyterLab is being served from the `/path/to/anaconda3/envs/` directory, which is where the virtual environments are stored)*:

```
[I 2022-09-07 09:31:23.835 LabApp] JupyterLab extension loaded from /path/to/anaconda3/envs/ds-marketing/lib/python3.9/site-packages/jupyterlab
[I 2022-09-07 09:31:23.835 LabApp] JupyterLab application directory is /path/to/anaconda3/envs/ds-marketing/share/jupyter/lab
```

2. When JupyterLab first loads up in your browser, under the "Launcher" tab you will see options for "Python 3 (ipykernel)" under the "Notebook" and "Console" headings. 

<br>

---

<br>

### How to update packages in a virtual environment

You may need to update your environment for a variety of reasons. For example, it may be the case that:

* One of your core dependencies just released a new version (dependency version number update).
* You need an additional package for data analysis (add a new dependency).
* You have found a better package and no longer need the older package (add new dependency and remove old dependency).

If any of these occur, all you need to do is update the contents of your `environment.yml` file accordingly and then run the following commands in your terminal:

```
conda deactivate

conda env update --file environment.yml --prune
```

#### Note

The `--prune` option causes conda to remove any dependencies that are no longer required from the environment.

See [Updating an environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#updating-an-environment).

<br>

### How to deactivate a virtual environment

When you are done working on a particular project you can deactivate the virtual environment with:

```
conda deactivate
```

See [Deactivating an environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#deactivating-an-environment)

<br>

### How to delete a virtual environment

First make sure your environment is deactivated. Then run this command:

```
conda env remove --name name-of-environment
```

You can verify that your virtual environment has been deleted by running

```
conda env list
```

<br>

---

<br>

## Install JupyterLab and run it outside of a virtual environment (NOT RECOMMENDED)

Install JupyterLab with `pip`:

```
pip install jupyterlab
```

Note: If you install JupyterLab with conda or mamba, we recommend using the conda-forge channel.

<br>

Check that JupyterLab installed correctly:

```
jupyter-lab --version
```

<br>

Once installed, launch JupyterLab with:

```
jupyter-lab
```
