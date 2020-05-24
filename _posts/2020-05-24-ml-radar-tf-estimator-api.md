# Week - 24th of May 2020 - Tensor Flow Estimator API

On this notes. I will delve a bit on interesting Tensor Flow components to make our lifes easier with a simple example.

To demonstrate the capabilities of the Estimator API we will use a toy example, problem.

"Create a neural network that is capable of finding the volume of a cylinder given the radius of 
its base (r) and its height (h). Assume that the radius and height of the cylinder
are both in the range 0.5 to 2.0."

## Producing the raw input data set

Adding basically a random dataframe dataset and splitting into 3 datasets training, validation and test.


```python

import math
import numpy as np
import pandas as pd 
df = pd.DataFrame(np.random.uniform(0.5,2,size=(10000, 2)), columns=list(['radian','height']))

df['volume'] = df.apply(lambda row: math.pi *(row.radian**2)*row.height, axis = 1) 


df_train, df_validate, df_test = np.split(df.sample(frac=1), [int(.6*len(df)), int(.8*len(df))])

```


## Adding estimator inputs

This basically involving wiring the input datasets above into a tensorflow compatible format, with all the scalability guarantees 
provided by the framework for datasets longer than the memory of the computing node.

FEATURES = ['radian','height']
LABEL = 'volume'

```python
def make_train_input_fn(df, num_epochs):
  return tf.estimator.inputs.pandas_input_fn(
    x = df,
    y = df[LABEL],
    batch_size = 128,
    num_epochs = num_epochs,
    shuffle = True,
    queue_capacity = 1000
  )


def make_eval_input_fn(df):
  return tf.estimator.inputs.pandas_input_fn(
    x = df,
    y = df[LABEL],
    batch_size = 128,
    shuffle = False,
    queue_capacity = 1000
  )


def make_prediction_input_fn(df):
  return tf.estimator.inputs.pandas_input_fn(
    x = df,
    y = None,
    batch_size = 128,
    shuffle = False,
    queue_capacity = 1000
  )


def make_feature_cols():
  input_columns = [tf.feature_column.numeric_column(k) for k in FEATURES]
  return input_columns


```

## Run a baseline model

A simple regressor is setup to baseline the accuracy of the model. 

```python

OUTDIR = 'volume_trained'
shutil.rmtree(OUTDIR, ignore_errors = True) # start fresh each time

model = tf.estimator.LinearRegressor(
      feature_columns = make_feature_cols(), model_dir = OUTDIR)

model.train(input_fn = make_train_input_fn(df_train, num_epochs = 10))
```

Now it's time to get some performance metrics of the initial model :

```python

def print_rmse(model, df):
  metrics = model.evaluate(input_fn = make_eval_input_fn(df))
  print('RMSE on dataset = {}'.format(np.sqrt(metrics['average_loss'])))

print_rmse(model, df_validate)

```

Output is the following :

```
...
INFO:tensorflow:Saving 'checkpoint_path' summary for global step 469: volume_trained/model.ckpt-469
RMSE on dataset = 3.5332114696502686

```

Sounds a like a pretty bad RMSE giving the range of the data generated :( . We need a Bazooka :  Let's deep learn this !!!!

## Run a deep learning model


```python

model = tf.estimator.DNNRegressor(hidden_units = [32, 8, 2],
      feature_columns = make_feature_cols(), model_dir = OUTDIR)
model.train(input_fn = make_train_input_fn(df_train, num_epochs = 100));
print_rmse(model, df_validate)

```
Output is the following after some minutes :

```
...
INFO:tensorflow:Finished evaluation at 2020-05-24-20:30:52
INFO:tensorflow:Saving dict for global step 4688: average_loss = 0.012107925, global_step = 4688, label/mean = 6.806968, loss = 1.5134906, prediction/mean = 6.8328032
INFO:tensorflow:Saving 'checkpoint_path' summary for global step 4688: volume_trained/model.ckpt-4688
RMSE on dataset = 0.11003601551055908

```

Definitely Deep Learning for the win in here, did run both of the models on the Google Cloud AI Platform / Notebooks.


## Comments

The estimator api seems to simplify a lot of the old plumbing needed in the Tensorflow to create the computation graph and prepare the 
data. Is definitely much more intuitive and allows for much more expressivity. 

Comparing with the Sklearn and the Keras Api i would say that the feeling is different but definitely from a user experience the 
similar level of easeness. I still think that sklearn with a native pandas integration allow for easy and dirty prototyping. 
The strength of Tensorflow is definitely production readiness and ability to scale when needed.









