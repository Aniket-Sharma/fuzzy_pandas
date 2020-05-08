# fuzzy_pandas

A razor-thin layer over [csvmatch](https://github.com/maxharlow/csvmatch/) that allows you to do fuzzy matching with pandas dataframes.

## Installation

```
pip install fuzzy_pandas
```

## Usage

To borrow 100% from the [original repo](https://github.com/maxharlow/csvmatch), say you have one CSV file such as:

```
name,location,codename
George Smiley,London,Beggerman
Percy Alleline,London,Tinker
Roy Bland,London,Soldier
Toby Esterhase,Vienna,Poorman
Peter Guillam,Brixton,none
```

And another such as:

```
Person Name,Location
Maria Andreyevna Ostrakova,Russia
Otto Leipzig,Estonia
George SMILEY,London
Peter Guillam,Brixton
Konny Saks,Oxford
```

You can then use `fdp.fuzzy_merge` to see which names are in both files:

```python
import pandas as pd
import fuzzy_pandas as fpd

df1 = pd.read_csv("data1.csv")
df2 = pd.read_csv("data2.csv")

fpd.fuzzy_merge(df1, df2,
                left_on=['name'],
                right_on=['Person Name'],
                ignore_case=True,
                keep='match')
```

|.|name|Person Name|
|---|---|---|
|0|George Smiley|George SMILEY|
|1|Peter Guillam|Peter Guillam|

That's a terrible, non-fuzzy example, though. Maybe you should hop to the next section if you want something more meaningful?

## Examples

You can find examples, including different types of matches (edit distance, phonetic, etc), [in this notebook from the examples folder](https://github.com/jsoma/fuzzy_pandas/blob/master/examples/fuzzy_pandas%20examples.ipynb).

## Options

All of these options can be sent as arguments to `fpd.fuzzy_merge`.

* **left** : DataFrame
* **right** : DataFrame
        - Object to merge `left` with
* **on** : `str` or `list`
        - Column names to compare. These must be found in both DataFrames.
* **left_on** : `str` or `list`
        - Column names to compare in the left DataFrame.
* **right_on** : `str` or `list`
        - Column names to compare in the right DataFrame.
* **keep** : str { 'all', 'match' }
        - Overrides `keep_left` and `keep_right`
* **keep_left** : `str` or `list`, default 'all'
        - List of columns to preserve from the left DataFrame.
        - If 'all', preserve all columns.
        - If 'match', preserve left_on matching) column.
        - If any other string, just keeps that one column.
* **keep_right** : `str` or `list`, default 'all'
        - List of columns to preserve from the right DataFrame.
        - If 'all', preserve all columns. Defaults to right_on.
        - If 'match', preserve right_on (matching) column.
        - If any other string, just keeps that one column.
* **method** : `str` or `list`, default 'exact'
        - Perform a fuzzy match, and an optional specified algorithm.
        - Multiple algorithms can be specified which will apply to each field respectively.
        - Options: 
          * **exact**: exact matches
          * **levenshtein**: string distance metric
          * **jaro**: string distance metric
          * **metaphone**: phoenetic matching algorithm
          * **bilenko**: prompts for matches
* **threshold** : float or list, default `0.6`
        - The threshold for a fuzzy match as a number between 0 and 1
        - Multiple numbers will be applied to each field respectively
* **ignore_case** : bool, default `False`
        - Ignore case (default is case-sensitive)
* **ignore_nonalpha** : bool, default `False`
        - Ignore non-alphanumeric characters
* **ignore_nonlatin** : bool, default `False`
        - Ignore characters from non-latin alphabets
        - Accented characters are compared to their unaccented equivalent
* **ignore_order_words** : bool, default `False`
        - Ignore the order words are given in
* **ignore_order_letters** : bool, default `False`
        - Ignore the order the letters are given in, regardless of word order
* **ignore_titles** : bool, default `False`
        - Ignore a predefined list of name titles (such as Mr, Ms, etc)
* **join** : { 'inner', 'left-outer', 'right-outer', 'full-outer' }
* **keep_degree** : bool, default `False`
	- Keep the match confindence in output dataframe in `degree` column

For more how-to information, check out [the examples folder](https://github.com/jsoma/fuzzy_pandas/tree/master/examples) or the [the original repo](https://github.com/maxharlow/csvmatch).
