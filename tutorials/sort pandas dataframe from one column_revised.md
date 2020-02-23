+++
title = "How to sort pandas dataframe by column"
date = 2020-02-24T09:43:59+05:00
draft = false
weight = 120
keywords = [""]
description = "This tutorial demonstrates  how to sort pandas dataframe by column."
tags = ["pandas", "Pandas Dataframe Sorting"]
author = "Asad Riaz"
postlink = 37787698
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



## `dataframe` Sorting Order - Argument `na_position`

Decides the position of NaNs after sorting i.e.  `last` puts NaNs at the end. Default value is `first`

Consider the following `dataframe`,

```python
import numpy as np
import pandas as pd
s = pd.Series([np.nan, 2, 4, 10, 7])
print(s.sort_values(na_position='last'))
```

After running the code we will get the following output. 

```
1     2.0
2     4.0
4     7.0
3    10.0
0     NaN
```



