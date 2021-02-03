- [High-resolution plot outputs for high resolution monitors](#high-resolution-plot-outputs-for-high-resolution-monitors)
- [Pretty display of multiple variables](#pretty-display-of-multiple-variables)
- [Execute notebook from command line](#execute-notebook-from-command-line)
- [In virtual environment mode](#in-virtual-environment-mode)
- [Change working dir for the notebook](#change-working-dir-for-the-notebook)
- [Magic stuff](#magic-stuff)
  - [Access python variable in a cell using bash magic](#access-python-variable-in-a-cell-using-bash-magic)


## High-resolution plot outputs for high resolution monitors

```sh
# insert this line
%config InlineBackend.figure_format = 'retina'
```


## Pretty display of multiple variables

By default, Jupyter pretty displays variable in the last line of a cell. Instead, if such pretty display is preferred for multiple variables despite their position inside

```py
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```

[Source](https://www.dataquest.io/blog/jupyter-notebook-tips-tricks-shortcuts/)


## Execute notebook from command line

```sh
# to just run the notebook
jupyter nbconvert --execute <notebook>

# replace existing notebook with processed output
jupyter nbconvert --execute --to notebook --inplace <notebook>

# If you want to run a notebook and produce a new notebook, use "--to notebook"
jupyter nbconvert --execute --to notebook <notebook>

# save as html (w/o executing)
jupyter nbconvert --to html <filename>

# if cell timeout is causing error, use --ExecutePreprocessor.timeout=None
jupyter nbconvert --to notebook --execute --ExecutePreprocessor.timeout=None --inplace <filename>
```

[Source](https://stackoverflow.com/a/35572827/3998252)

To get around cell timeout when running from commandline. use "--ExecutePreprocessor.timeout=None".


## In virtual environment mode

Use of Jupyter notebook in virtual environment is easy to manage via `pipenv`

```sh
pipenv install jupyter

# to open a session
pipenv run jupyter notebook

# to run them in commandline via nbconvert
## install extensions
pipenv install jupyter_contrib_nbextensions

## run nbconvert
pipenv run jupyter nbconvert --<other_flags/params>
```


## Change working dir for the notebook

```py
import os
os.chdir('..')
print (f"Working dir: {os.getcwd()}")
```

[Source](https://stackoverflow.com/a/35665295/3998252)

Note: `%cd` line magic can be used as well.

## Magic stuff

Some interesting magics:

| Magic    |                       Purpose                       |
| -------- | :-------------------------------------------------: |
| %%bash   |        Run cells with bash in a subprocess.         |
| %%script |           Run a cell via a shell command            |
| !      |        To run a shell command        |
| %cd      |        Change the current working directory.        |
| %env     |      Get, set, or list environment variables.       |
| %run     |   Run the named file inside IPython as a program.   |
| %time    | Time execution of a Python statement or expression. |


### Access python variable in a cell using bash magic

```
%%bash -s "$myPythonVar" "$myOtherVar"
echo "This bash script knows about $1 and $2"
```

[Source](https://stackoverflow.com/a/19674648/3998252)

