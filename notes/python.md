- [pandas](#pandas)
    - [Print without breaking rows/columns](#print-without-breaking-rowscolumns)
    - [Replace NaN in one column with value from corresponding row of second column](#replace-nan-in-one-column-with-value-from-corresponding-row-of-second-column)
    - [Identify rows with NaN values](#identify-rows-with-nan-values)
    - [Conditionally replace values](#conditionally-replace-values)
    - [Remove columns](#remove-columns)
    - [Splitting column into multiple columns](#splitting-column-into-multiple-columns)
    - [Extract strings by position into new column](#extract-strings-by-position-into-new-column)
    - [Dataframe in percentage](#dataframe-in-percentage)
    - [Sorting multilevel dataframe (pivot table)](#sorting-multilevel-dataframe-pivot-table)
    - [Adding new column with mapped value from a dictionary](#adding-new-column-with-mapped-value-from-a-dictionary)
    - [Make dictionary from Pandas columns](#make-dictionary-from-pandas-columns)
    - [Convert dataframe to file object](#convert-dataframe-to-file-object)
    - [Rename column name](#rename-column-name)
    - [Ignore commented lines when reading files](#ignore-commented-lines-when-reading-files)
    - [Reading massive files in chunks](#reading-massive-files-in-chunks)
    - [Checking if item is NaN or not:](#checking-if-item-is-nan-or-not)
- [Charts](#charts)
    - [Creating reproducible charts](#creating-reproducible-charts)
    - [Why use fig, ax = plt.subplots():](#why-use-fig-ax--pltsubplots)
    - [Change figure parameters globally for all figures in a script](#change-figure-parameters-globally-for-all-figures-in-a-script)
    - [Legends for chart with two Y-axes](#legends-for-chart-with-two-y-axes)
    - [Force axis tick labels to be integers](#force-axis-tick-labels-to-be-integers)
    - [xticks and xtick_labels](#xticks-and-xticklabels)
    - [importing matplotlib in cluster](#importing-matplotlib-in-cluster)
    - [Axis in exponential format](#axis-in-exponential-format)
    - [Legend positioning without crowding](#legend-positioning-without-crowding)
- [Virtual environment](#virtual-environment)
    - [pipenv](#pipenv)
- [pytest](#pytest)
    - [Avoiding .pyc files created when running pytest](#avoiding-pyc-files-created-when-running-pytest)
    - [Disable per-test capturing](#disable-per-test-capturing)
    - [Parametrizing test functions](#parametrizing-test-functions)
    - [Coverage](#coverage)


# pandas

## Print without breaking rows/columns
```
import pandas as pd
pd.set_option('display.height', 1000)
pd.set_option('display.max_rows', 500)
pd.set_option('display.max_columns', 500)
pd.set_option('display.width', 10000)
```
[Source](https://stackoverflow.com/a/11711637/3998252).


## Replace NaN in one column with value from corresponding row of second column
```
df['colA'].fillna(df['colB'], inplace=True)
```
[Source](https://stackoverflow.com/a/29177664/3998252)


## Identify rows with NaN values
```
df[df['a'].isnull()]
```
[Source](https://stackoverflow.com/a/38368514/3998252)


## Conditionally replace values

```
# Method 1; See https://stackoverflow.com/a/21608417/3998252
df.ix[df['A'] > 20000, 'A'] = 0
 
# Method 2; See https://stackoverflow.com/a/19913845/3998252
import numpy as np
df['A'] = np.where(df['B']=='Z', 1, 0)
 
# Method 3; See https://stackoverflow.com/a/31173785/3998252
df['A'] = [1' if x == 'Z' else 0 for x in df['B']]
```


## Remove columns
```
# to get into a new dataframe
df = df.drop('column_name', axis=1)
 
# to remove it inplace
df.drop('column_name', axis=1, inplace=True)
```
[Source](https://stackoverflow.com/a/18145399/3998252)


## Splitting column into multiple columns

```
# create a df
> df = pd.DataFrame(data={'aaa':['a 2 3','b 6 7 8']})
> df
       aaa
0    a 2 3
1  b 6 7 8
 
# to keep 1 item in a column and store remaining in another column
> df['x'], df['y'] = df['aaa'].str.split(' ', 1).str
> df[['x', 'y']]
   x      y
0  a    2 3
1  b  6 7 8
 
# split column into multiple columns by a delimiter
> df = df['aaa'].str.split(' ', expand=True)
> df
   0  1  2     3
0  a  2  3  None
1  b  6  7     8
```
[Source](https://stackoverflow.com/a/39358924/3998252)


## Extract strings by position into new column
```
data_pd['new_col'] = data_pd['col_X'].str[:1]
```
[Source](https://stackoverflow.com/a/20970328/3998252)


## Dataframe in percentage
```
# if by row
df.apply(lambda  x: x / x.sum() * 100, axis=1)
 
# if by column
df.apply(lambda  x: x / x.sum() * 100, axis=0)
```


## Sorting multilevel dataframe (pivot table)
```
    Group1    Group2
    A B C     A B C
1   1 0 3     2 5 7
2   5 6 9     1 0 0
3   7 0 2     0 3 5
 
# Sorted by column 'C' of 'Group1'. They need to be in a TUPLE.
df.sort([('Group1', 'C')], ascending=False)
 
  Group1       Group2     
       A  B  C      A  B  C
2      5  6  9      1  0  0
1      1  0  3      2  5  7
3      7  0  2      0  3  5
```
[Source](https://stackoverflow.com/a/14734148/3998252)


## Adding new column with mapped value from a dictionary
```
df["col_2"] = df["col_1"].map(dict_name)
```

[Source](https://stackoverflow.com/a/24216489/3998252)

If desired to replace existing column with dictionary value:
df.replace({"col_1": dict_name})

[Source](https://stackoverflow.com/a/20250996/3998252)


## Make dictionary from Pandas columns
```
# works only if 1:1 key to value pairing
dict_name = df.set_index('key_col')['value_col'].to_dict()
```
[Source](https://stackoverflow.com/a/18695700/3998252)


## Convert dataframe to file object
```
from cStringIO import StringIO
 
xx = StringIO()
data_pd.to_csv(xx, index=False, sep='\t')
xx.seek(0)  # this is important; position set to the beginning
```
[Source](https://stackoverflow.com/a/38213524/3998252)


## Rename column name
```
df2 = df.rename(columns={'old_name' : 'new_name'})
 
# in-place
df2.rename(columns={'old_name' : 'new_name'}, inplace = True)
```


## Ignore commented lines when reading files
Set comment='#' when using pd.read_csv


## Reading massive files in chunks

Use `iterator` option for iterating file in chunks
```
chunksize= 10**6
for df in pd.read_csv('filename.gz',sep='\t', header=None, chunksize=chunksize, iterator=True):
    print df[0].value_counts()
```
[See here for more info on this](http://pythondata.com/working-large-csv-files-python/).


## Checking if item is NaN or not:
```
# using numpy
numpy.isnan(item)   # item can't be string dtype
 
# using math
math.isnan(item)    # item can't be string dtype
 
# using pandas
pandas.isnull(item) # allows string dtype
```


# Charts

## Creating reproducible charts
http://www.jesshamrick.com/2016/04/13/reproducible-plots/


## Why use fig, ax = plt.subplots():
[See here](https://stackoverflow.com/a/34162641/3998252)


## Change figure parameters globally for all figures in a script
[See this](http://stackoverflow.com/a/38251497/3998252).
See this page for parameters that can be modified -http://matplotlib.org/users/customizing.html.


## Legends for chart with two Y-axes

```
fig, ax1 = plt.subplots()   # primary
ax2 = ax1.twinx()   # for second y-axis
 
ax1.bar(x,y)
ax2.bar(x,y)
 
# for legend
plots1, labels1 = ax.get_legend_handles_labels()
plots2, labels2 = ax2.get_legend_handles_labels()
ax1.legend(plots1 + plots2, labels1 + labels2)
```
[Source](http://stackoverflow.com/a/10129461/3998252)


## Force axis tick labels to be integers

```
from matplotlib.ticker import MaxNLocator
....
ax.yaxis.set_major_locator(MaxNLocator(integer=True))
```
[Source](https://stackoverflow.com/a/38096332/3998252)
[Documentation](https://matplotlib.org/api/ticker_api.html#matplotlib.ticker.MaxNLocator)


## xticks and xtick_labels
```
ax.set_xticks(x_axis)
ax.set_xticklabels(x_labels, rotation=45, ha='right')

# when using seaborn where x_labels were preset, for example-seaborn
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```


## importing matplotlib in cluster
```
import matplotlib as mpl
mpl.use('Agg')
import matplotlib.pyplot as plt
```
[Source](https://stackoverflow.com/a/13336944/3998252)


## Axis in exponential format
```
ax.ticklabel_format(style='sci', axis='y', scilimits=(0, 0))
```


## Legend positioning without crowding

Method 1 (Preferred):
```
lgd = ax.legend(bbox_to_anchor=(0., -0.3, 1., .102), loc=3, ncol=3, mode="expand", borderaxespad=0.)
fig.savefig('a.png', bbox_extra_artists=(lgd,), bbox_inches='tight')
```
[Source](https://stackoverflow.com/a/10154763/3998252)

Method 2:
```
# Shrink current axis by 20%; to accomodate legends.
box = ax.get_position()
ax.set_position([box.x0, box.y0, box.width * 0.85, box.height])
    
# change 'bbox_to_anchor' for legend positions
handles, labels = ax.get_legend_handles_labels()
ax.legend(handles, labels, loc='center left', bbox_to_anchor=(1, 0.5))
```

 

# Virtual environment

## pipenv

pipenv is the easiest way to manage virtual environment as it automates several easy-to-forget-but-required steps. It requires Python 3 installedthough.

a. Creating new/fresh virtual environment
```
cd project_dir
 
# to initiate pipenv virtual env
pipenv install
 
# to install required packages
pipenv install [packages]
 
# **IMPORTANT** To LOCK pipfile with EXACT version info
pipenv lock
 
# activate virtual environment
pipenv shell
 
# exit virtual environment
exit
```
 

b. Recreating virtual environment from Pipfile
```
# installs all packages from Pipfile
pipenv install
 
# activate virtual environment
pipenv shell
 
# exit virtual environment
exit
```
 

c. Opening existing pipenv project
```
cd project_dir
 
# activate virtual environment
pipenv shell
 
# exit virtual environment
exit
```
 

d. Remove virtual environment
```
cd <project_dir>
pipenv --rm
```
 

e. Simply running a python script with pipenv without spawning a new shell
```
pipenv run python script.py
```
 



 # pytest

pytest can't seem to handle if file to tested has its name starting with a number. For example, filename '1_liftover_hg38_to_hg19.py' caused trouble and then was changed to 'a_liftover_hg38_to_hg19.py'. This is probably due to importing module with number as first letter causing issues.


## Avoiding .pyc files created when running pytest

http://stackoverflow.com/questions/154443/how-to-avoid-pyc-files/154617#154617

Can be done via terminal or programmatically.


## Disable per-test capturing

Use -s switch.

pytest -s

 
## Parametrizing test functions

https://docs.pytest.org/en/latest/parametrize.html

Example:
```
import pytest
@pytest.mark.parametrize("test_input,expected", [
    ("3+5", 8),
    ("2+4", 6),
    ("6*9", 42),
])
def test_eval(test_input, expected):
    assert eval(test_input) == expected
```
 
## Coverage

https://pytest-cov.readthedocs.io/en/latest/readme.html#id1

Simple execution:
`py.test --cov=myproj tests/`

Generate reports:
`py.test --cov-report term --cov=myproj tests/`

--cov-report takes values html, xml, term and annotate
