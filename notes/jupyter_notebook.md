- [High-resolution plot outputs for high resolution monitors](#high-resolution-plot-outputs-for-high-resolution-monitors)
- [Pretty display of multiple variables](#pretty-display-of-multiple-variables)
- [Execute notebook from command line](#execute-notebook-from-command-line)


## High-resolution plot outputs for high resolution monitors
```
# insert this line
%config InlineBackend.figure_format = 'retina'
```


## Pretty display of multiple variables

By default, Jupyter pretty displays variable in the last line of a cell. Instead, if such pretty display is preferred for multiple variables despite their position inside
```
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```
[Source](https://www.dataquest.io/blog/jupyter-notebook-tips-tricks-shortcuts/)


## Execute notebook from command line
```
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




