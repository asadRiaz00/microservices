+++
title = how to sort pandas dataframe by column"
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

We will introduce methods to sort the dataframes records by column,

Consider the following dataframe

```python
import pandas as pd
df = pd.DataFrame({
    'col1': ['A', 'A', 'B', 'C', 'D', 'C'],
    'col2': [2, 1, 9, 8, 7, 4],
    'col3': [0, 1, 9, 4, 2, 3],
})
print(df)
```
If you run the abovethe code you will get the output as following 

```
 col1  col2  col3
0    A     2     0
1    A     1     1
2    B     9     9
3    C     8     4
4    D     7     2
5    C     4     3
```
