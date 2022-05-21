[原文](https://stackoverflow.com/a/50194143)
# Solution without sys.path hacks
## Summary
 - Wrap the code into one folder (e.g. packaged_stuff)
 - Create a setup.py script where you use [setuptools.setup()](https://stackoverflow.com/a/50193944/3015186).
 - Pip install the package in editable state with pip install -e <myproject_folder>
 - Import using from packaged_stuff.modulename import function_name
## Setup
I assume the same folder structure as in the question
```bash
.
└── ptdraft
    ├── __init__.py
    ├── nib.py
    └── simulations
        ├── __init__.py
        └── life
            ├── __init__.py
            └── life.py
```
I call the . the root folder, and in my case it is located in C:\tmp\test_imports.

## Steps
1. Add a setup.py to the root folder -- The contents of the setup.py can be simply

```python
    from setuptools import setup, find_packages

    setup(name='myproject', version='1.0', packages=find_packages())
```
Basically "any" setup.py would work. This is just a minimal working example.

2. Use a virtual environment
If you are familiar with virtual environments, activate one, and skip to the next step. Usage of virtual environments are not absolutely required, but they will really help you out in the long run (when you have more than 1 project ongoing..). The most basic steps are (run in the root folder)

Create virtual env
`python -m venv venv`
Activate virtual env
`. venv/bin/activate` (Linux) or `./venv/Scripts/activate (Win)`
Deactivate virtual env
`deactivate (Linux)`
To learn more about this, just Google out "python virtualenv tutorial" or similar. You probably never need any other commands than creating, activating and deactivating.

Once you have made and activated a virtual environment, your console should give the name of the virtual environment in parenthesis
```bash
PS C:\tmp\test_imports> python -m venv venv
PS C:\tmp\test_imports> .\venv\Scripts\activate
(venv) PS C:\tmp\test_imports>
```
3. pip install your project in editable state
Install your top level package myproject using pip. The trick is to use the -e flag when doing the install. This way it is installed in an editable state, and all the edits made to the .py files will be automatically included in the installed package.

In the root directory, run

`pip install -e .` (note the dot, it stands for "current directory")

You can also see that it is installed by using `pip freeze`
```
(venv) PS C:\tmp\test_imports> pip install -e .
Obtaining file:///C:/tmp/test_imports
Installing collected packages: myproject
  Running setup.py develop for myproject
Successfully installed myproject
(venv) PS C:\tmp\test_imports> pip freeze
myproject==1.0
```
Import by prepending `mainfolder` to every import
In this example, the `mainfolder` would be `ptdraft`. This has the advantage that you will not run into name collisions with other module names (from python standard library or 3rd party modules).

# Example Usage
## nib.py
```python
def function_from_nib():
    print('I am the return value from function_from_nib!')
```
## life.py
```python
from ptdraft.nib import function_from_nib

if __name__ == '__main__':
    function_from_nib()
```

## Running life.py
```
(venv) PS C:\tmp\test_imports> python .\ptdraft\simulations\life\life.py
I am the return value from function_from_nib!
```
