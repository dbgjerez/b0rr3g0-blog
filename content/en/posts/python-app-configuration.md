---
title: "Python application configuration"
date: 2022-07-13
draft: true
tags: ["python", "cloud-native", "twelve-factor"]
type: post
---
Nowadays, we have to configure many configuration parameters in our applications. These parameters sometimes depend on the execution environment. 

We need to build a maintainable code whose configuration should be externalised. This way, we can run the same package in different environments. More information [here](https://12factor.net/config)
<!--more-->
Another important concept is "cloud-native". This concept refers to how we should develop our applications, facilitating running and configuring them in cloud environments. 

In this article, we are going to see how **to configure our Python applications in a "cloud-native" way**.

> In addition, exist other ways to configure parameters as secrets. These cover the secret's lifecycle and how it is provided, which should be independent that our programming language. 

# Environment variables

The environment variables are a basic way to externalise the configuration. 

To read the variables in our Python code, we will use the ```os``` library.

```python
import os

user = os.getenv('USER')
print("[USER]:", user)
```

Run it:

```zsh
❯ python tmp.py
[USER]: db
```

# Configuration files

The configuration files have the advantage to manage many variables easily. These variables are in one or more files that can be replaced depending on the environment. 

The ```python-dotenv``` library helps us to load the configuration files. By default, the library loads a (K, V) properties file, but we can change its format. 

Firstly, install the library:

```zsh
pip install python-dotenv
```

Now, we only have to use it to load the configuration:

```python
from dotenv import load_dotenv
import os

load_dotenv(".env", override=True)

user = os.getenv('USER')
print("[USER]:", user)
```

The function ```load_dotenv``` can be configured:
* file: by default, the library searches the file ```.env```, but we can indicate another. 
* Override: this property indicates if we like to give priority the file variables over system variables. 

Create the ```.env``` file:

```zsh
❯ echo 'USER=dborrego' > .env
```

And, run the Python code: 

```zsh
❯ python tmp.py
[USER]: dborrego
```

# References

* The twelve-factor app: https://12factor.net/
* https://pypi.org/project/python-dotenv/