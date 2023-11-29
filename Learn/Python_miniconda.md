

### 1. install the conda program on linux

to install folow this steps:

```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
````

After installing, initialize your newly-installed Miniconda. The following commands initialize for bash and zsh shells:

````
~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
````


You can find them here:

https://docs.conda.io/projects/miniconda/en/latest/#quick-command-line-install


Test to see if it is working:  open a new shell and run:

```
conda info --envs
```

### 2. create the python environment 

you can create the environment using 

```
conda create --name <env_name> python==X.X.X
```

example:

```
conda create --name myapp python==X.X.X
```

NOTE: this will create the environment in a default path.

If you want the environemtn to be created in a specific path, use instead


```
conda create --prefix /path/to/myapp/pyhton_env  python=3.11
```

### 3. Activate the enviroment just created to install dependecies

```
conda activate /path/to/myapp/pyhton_env
```

### 4. Install dependecies

Once activated the env (see 3):

run 

```
pip install <library/dependency/package>
```

example

```
pip install pdftotext
```

To see all installaed dependencies in your environment, run

```
conda list
```

Keep installing all dependecies found in the file:

```
requirements.txt
```

### OPTIONAL: if you have to use python in a exec,  set the path of the pyton binary in your .env file

set the key:

PYTHON_VIRTUAL_ENV

with the fullpath of the python binary in your virtual env.

Example:

```
PYTHON_VIRTUAL_ENV=/path/to/myapp/pyhton_env/bin/python
```

### REMEMBER:

- always specify your python version when creating a new conda environment;
not doing so can cause all sorts of issues down the line with package and dependency conflicts.

- If you start adding packages with pip, keep using them with pip. You can also use 'conda-forge' to install to add packages to your conda environment (for example: conda install -c conda-forge jupyterlab ) . 
It is recommended that you avoid using conda and pip interchangeably within the same conda environment because some of the package versions that reside within each one of those “channels” can contain minor differences.
This doesn’t mean you can’t install a few packages from pip, but you should exercise caution.


### RESOURCES

Miniconda docs: https://docs.conda.io/projects/miniconda/en/latest/
Minicodan command line install: https://docs.conda.io/projects/miniconda/en/latest/#quick-command-line-install
Setting up Miniconda for Beginner Data Scientists - https://eduand-alvarez.medium.com/setting-up-anaconda-on-your-windows-pc-6e39800c1afb
