Title: Python Virtual Environment
Date: 2019-10-20 16:00
Modified: 2019-10-20 16:30
Category: python
Tags: python
Slug: python-virtual-environment
Authors: Jack

In python development, virutal environment is very useful for seperating python running environment.  For example different, different versions of python or packages can be applied in different virtual environment.  As a result, developers can easily switch among python virtual environment in a single machine.  Based on requirement, virtual environment can be rebuild by providing package requirements.
Let me show you how to do it in python3.

### Create Virtual Environment

```
mkdir [my-projetc]
cd [my-project]
python3 -m venv [venv]
```
replace [venv] with a desired name of you virtual environment

### Activate Virtual Environment

```
source ./[venv]/bin/activate
```

### Install Package with pip

```
pip install [requests]
```

### Export Installed Package List to requirements.txt

```
pip freeze > requirements.txt
```

### De-activate Virtual Environment

```
deactivate
```

### In .gitignore file add the path of virtual environment

```
[venv]/
```

 You don't need virutal environment in version controll, because you have them all in requirements.txt. You can re-create virtual environment with it.

### Re-create Virtual Environment

```
cd [your-project]
python3 -m venv [venv]
source ./[venv]/bin/activate
pip install -r requirements.txt

```
