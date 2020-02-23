+++
title = "How to sort pandas dataframe by column"
date = 2019-11-24T13:32:59+01:00
draft = false
weight = 120
keywords = [""]
description = "This tutorial demonstrates  how to sort pandas dataframe by column."
tags = ["pandas", "Pandas Dataframe Sorting"]
author = "Jinku Hu"
postlink = 12364981
inarticle = true

+++

We will introduce method `pandas.DataFrame.sort_values` to sort the dataframes values.

Consider the following `dataframe`,

```python
import pandas as pd
df = pd.DataFrame({
    'col1': ['g', 't', 'n', 'w', 'n', 'g'],
    'col2': [5, 2, 5, 1, 3, 6],
    'col3': [0, 7, 2, 8,1, 2],
})
print(df)
```

If you run this code you will get the output as following which is not sorted yet.

```
    col1  col2  col3
0    g     5     0
1    t     2     7
2    n     5     2
3    w     1     8
4    n     3     1
5    g     6     2
```

Now we could sort the `dataframe` with the codes below. 

```python
import pandas as pd
df = pd.DataFrame({
    'col1': ['g', 't', 'n', 'w', 'n', 'g'],
    'col2': [5, 2, 5, 1, 3, 6],
    'col3': [0, 7, 2, 8,1, 2],
})
print(df.sort_values(by=['col1']))
```

We sorted `dataframe` by `col1`. After running the above code you will get the following output.

```
    col1  col2  col3
0    g     5     0
5    g     6     2
2    n     5     2
4    n     3     1
1    t     2     7
3    w     1     8
```

We can use more than one column for sorting as well, let change the last line of above code as follow,

```python
print(df.sort_values(by=['col1','col2']))
```
After running the code we will get the following output. 

```
    col1  col2  col3
0    g     5     0
5    g     6     2
4    n     3     1
2    n     5     2
1    t     2     7
3    w     1     8
```

Now  `dataframe` is further sorted by `col2` as well. 

## `dataframe` Sorting Order - Argument `ascending`

By default sorting is in ascending order, to change `dataframe` in descending order, we need to set flag  `ascending=false`.

```python
print(df.sort_values(by=['col1','col2'], ascending=False))
```

After running the code we will get the following output. 

```
    col1  col2  col3
3    w     1     8
1    t     2     7
2    n     5     2
4    n     3     1
5    g     6     2
0    g     5     0
```

