

## VIRTUAL ENV

#### why virtual envs? 
to avoid libraries conflicting with each other for different projects, also to avoid conflicts with package managers in some systems like unix 

- to create a new virtual env run:

```shell
python -m venv <path/to/dir/where/install/virtual/env>
```

NOTE: if you have more than one installation of python in your machine use python3 or python2 according to the version that you want

- then activate the virual env

```shell
cd <path/to/dir/where/install/virtual/env>
source bin/activate
```

NOTE: you will see your shell with a prefix of the dir where you installed your virtual env. example, suppose a installation in a .venv folder (which by the way is the standard)
then yoour shell will be like (.venv) [root@30d9138e737c ~]

- check the installed packages in the env

```shell
pip list
```

- exit from virtual env

```shell
deactivate
```

resources: https://docs.python.org/3/tutorial/venv.html
