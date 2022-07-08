---
title: "Virtual environments with Python"
date: 2022-07-08
draft: false
tags: ["python", "virtualenv"]
---

Working with Python we have a lot of libraries or dependencies.

Our projects use just a version of each dependency. It's very important to decouple the list of dependencies between projects. Otherwise, our applications could have an undesired behaviour.
<!--more--> 
Los entornos virtuales de Python nos ayudan en dicha tarea.

Prerequisites:
* Python
* Pip

# Virtual environment

We will use the command ```virtualenv``` to work with virtual environments in Python. 

If you have not installed "virtualenv", install it:

```zsh
❯ pip install virtualenv
```

Creation of an environment called ```venv```

```zsh
❯ python -m venv venv
```

To use the environment:

```zsh
❯ source venv/bin/activate
```

Now, we will list the dependencies in our environment, we should have only the basics of a new installation:

```zsh
❯ pip list
Package    Version
---------- -------
pip        21.2.3
setuptools 57.4.0
```

# Dependencies file

The file ```requeriments.txt``` has the list of dependencies necessary to run our application. 

## How to create the file

The easiest way is to start a new environment and install all the needed dependencies. In this demo, I will install ```pandas```.

```zsh
❯ pip install pandas
```

If we list now the libraries, we can see all of them: 

```zsh
❯ pip list
Package         Version
--------------- -------
numpy           1.23.0
pandas          1.4.3
pip             21.2.3
python-dateutil 2.8.2
pytz            2022.1
setuptools      57.4.0
six             1.16.0
```

When we have the complete list installed, we will use the ```freeze``` command to generate the file:

```zsh
❯ pip freeze > requirements.txt

❯ cat requirements.txt 
numpy==1.23.0
pandas==1.4.3
python-dateutil==2.8.2
pytz==2022.1
six==1.16.0
```

## How to install the dependencies

Using the command ```pip install```, we can install all the dependencies indicated in our file:

```zsh
❯ pip install -r requirements.txt
```

# Close the virtual env

To close the virtual environment in Python we only have to use the command ```deactivate```.

```zsh
❯ deactivate 
```