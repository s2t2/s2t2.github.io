---
layout: post
title:  "Learning Python"
author: MJ Rossetti
categories: process-documentation
tags: python pip
published: false
icon_class: snake
technologies: python pip
credits:  
 - http://stackoverflow.com/questions/8726207/what-are-the-python-equivalents-to-rubys-bundler-perls-carton
 - http://pip.readthedocs.org/en/stable/user_guide/#installing-packages
 - http://pip.readthedocs.org/en/stable/reference/pip_install/#requirements-file-format
 - https://pip.readthedocs.org/en/1.1/requirements.html#the-requirements-file-format
 - http://stackoverflow.com/questions/510972/getting-the-class-name-of-an-instance-in-python
 - https://docs.python.org/2/library/stdtypes.html#mapping-types-dict
 - https://docs.python.org/2/library/stdtypes.html#sequence-types-str-unicode-list-tuple-bytearray-buffer-xrange
 - https://developers.google.com/edu/python/dict-files?hl=en
---

Create a new project directory.

```` sh
mkdir ~/Desktop/myApp
cd ~/Desktop/myApp
````

Specify package dependencies, if necessary.

Add a file called **requirements.txt** to the root directory. Write the name of each required python package dependency on a new line, save the file, and exit.

    _________
    _________
    _____

Install package dependencies, if necessary.

```` sh
pip install -r requirements.txt
python setup.py install
````

Add a file called **my_script.py** to the root directory.

    # my_script.py
    print("DOING WORK HERE")

Run it.

```` sh
python my_script.py
````

Debug it as necessary.

```` py
# https://gist.github.com/obfusk/208597ccc64bf9b436ed
import code
code.interact(local=locals())
````

Detect classes of objects.

```` py
type(my_obj) # or...
my_obj.__class__.__name__
````

Detect class methods and documentation.

```` py
dir(my_obj)
help(my_obj)
````

## Python Object Types

`dict`, or dictionary, is like a hash

`u`, or unicode, is like a string

Convert `dict` to json object:

```` py
json.dumps(my_obj)
````
