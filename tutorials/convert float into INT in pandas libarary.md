+++
title = how to convert a float to an integer in a Pandas DataFrame"
date = 2019-11-24T13:32:59+01:00
draft = false
weight = 120
keywords = [""]
description = "This tutorial demonstrates  to convert a float to an integer in a Pandas DataFrame."
tags = ["", ""]
author = "Jinku Hu"
postlink = 12364981
inarticle = true

+++

We will  demonstrates method   to convert a float to an integer in a Pandas DataFrame.

First we will create random array using `numpy` libarary and then convert it into `Dataframe`

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.rand(5, 5) * 5)
print(df)
```
If you run this  code you will get the output as following which have values in `float`.

```
          0         1         2         3         4
0  0.302448  3.551958  3.878660  2.380352  4.741592
1  4.054187  0.940952  0.459058  4.314801  0.524993
2  2.891733  4.926885  4.955773  2.626373  4.144166
3  1.127639  3.196823  4.144020  1.350632  0.401138
4  1.423537  2.019455  3.038945  0.436657  3.823888
```
---
**NOTE**

Value will be different every time we run the code because we are generating random values here.

---

To convert float into INT we will use `astype(int)` method which is provided by `Pandas` package.Cange the code as follow

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.rand(5, 5) * 5)
print('*********** Random Float DataFrame ************')
print(df)
print('***********************************************')
print('***********************************************')
print('*********** Dataframe Converted into INT ************')
print(df.astype(int))
print('***********************************************')
print('***********************************************')
```

 After running above module we will get the following output.

```
*********** Random Float DataFrame ************
          0         1         2         3         4
0  1.510618  1.094561  4.157419  4.424195  4.872719
1  0.457680  4.002959  2.660999  1.686916  0.840159
2  1.781778  3.812924  0.561827  0.532328  0.752800
3  1.456514  2.774955  2.700118  4.503354  4.749377
4  2.223520  4.859238  0.450977  3.228444  2.541648
***********************************************
***********************************************
*********** Dataframe Converted into INT ************
   0  1  2  3  4
0  1  1  4  4  4
1  0  4  2  1  0
2  1  3  0  0  0
3  1  2  2  4  4
4  2  4  0  3  2
***********************************************
***********************************************
```

We can roundof the float value to int by using  `df.round(0).astype(int)`.

```python
import pandas as pd
import numpy as np
df = pd.DataFrame(np.random.rand(5, 5) * 5)
print('*********** Random Float DataFrame ************')
print(df)
print('***********************************************')
print('***********************************************')
print('*********** Dataframe Converted into INT ************')
print(df.astype(int))
print('***********************************************')
print('***********************************************')
print('*********** Rounding Float value to INT ************')
print(df.round(0).astype(int))
print('***********************************************')
print('***********************************************')

```

After running the code we will get the following output. 

```
*********** Random Float DataFrame ************
          0         1         2         3         4
0  3.394458  0.071202  3.313205  4.785878  0.705612
1  1.960353  0.842085  1.212332  0.818343  2.637940
2  4.407811  4.239078  1.419027  1.526310  1.043394
3  3.654216  4.672972  1.796439  3.168426  0.734009
4  0.848796  1.024052  1.859319  0.844378  1.747628
***********************************************
***********************************************
*********** Dataframe Converted into INT ************
   0  1  2  3  4
0  3  0  3  4  0
1  1  0  1  0  2
2  4  4  1  1  1
3  3  4  1  3  0
4  0  1  1  0  1
***********************************************
***********************************************
*********** Rounding Float value to INT ************
   0  1  2  3  4
0  3  0  3  5  1
1  2  1  1  1  3
2  4  4  1  2  1
3  4  5  2  3  1
4  1  1  2  1  2
***********************************************
***********************************************
```


```
