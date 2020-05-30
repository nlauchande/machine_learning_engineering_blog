# Introduction to ML for Trading - Part I 

On this notes. I will delve a bit on my hobby interest on trading systems as a way to exercise my passion for Machine Learning and 
Software Engineering. One of my multiple dangling hobby coding projects is : https://github.com/nlauchande/bitpred , i will use this
series of notes to explore some techniques and understand how to properly improve on bitpred.

## Goals of the post
Introducing the general concepts of :

1. Trading Strategy

2. Backtesting

3. ML Examples


## Trading Strategy
.....

```python

import math
import numpy as np
import pandas as pd 
df = pd.DataFrame(np.random.uniform(0.5,2,size=(10000, 2)), columns=list(['radian','height']))

df['volume'] = df.apply(lambda row: math.pi *(row.radian**2)*row.height, axis = 1) 


df_train, df_validate, df_test = np.split(df.sample(frac=1), [int(.6*len(df)), int(.8*len(df))])

```

## Backtesting
.....

```python

import math
import numpy as np
import pandas as pd 
df = pd.DataFrame(np.random.uniform(0.5,2,size=(10000, 2)), columns=list(['radian','height']))

df['volume'] = df.apply(lambda row: math.pi *(row.radian**2)*row.height, axis = 1) 


df_train, df_validate, df_test = np.split(df.sample(frac=1), [int(.6*len(df)), int(.8*len(df))])

```

## ML Examples
.....

```python

import math
import numpy as np
import pandas as pd 
df = pd.DataFrame(np.random.uniform(0.5,2,size=(10000, 2)), columns=list(['radian','height']))

df['volume'] = df.apply(lambda row: math.pi *(row.radian**2)*row.height, axis = 1) 


df_train, df_validate, df_test = np.split(df.sample(frac=1), [int(.6*len(df)), int(.8*len(df))])

```


## Comments










