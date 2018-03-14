### Basic tensorflow functions and commands

#### 1. Array operations
- [__`tf.multinomial( logits, num_samples )`__](https://www.tensorflow.org/api_docs/python/tf/multinomial)
   1. __`logits`__ 2-D Tensor with shape [batch_size, num_classes]. Each slice [i, :] represents the unnormalized log-probabilities for all classes.
   2. __`num_samples`__ 0-D. Number of independent samples to draw for each row slice.
```buildoutcfg
    # samples has shape [1, 5], where each value is either 0 or 1 with equal
    # probability.
    samples = tf.multinomial(tf.log([[10., 10.]]), 5)
```


- [__`tf.to_float()`__]()

- [__`tf.shape()`__](https://www.tensorflow.org/api_docs/python/tf/shape) this is the op to get the shape of a tensor.

- [__`tf.reduce_mean(input_tensor, axis)`__](https://www.tensorflow.org/api_docs/python/tf/reduce_mean) axis ranges from -rank(input_tensor) to rank(input_tensor). 
axis = 0, 1, 2 correspond to outer most bracket dimension, the second outer most bracket dimension, etc.  
```buildoutcfg
    x = tf.constant([[1., 1.], [2., 2.]])
    tf.reduce_mean(x)  # 1.5
    tf.reduce_mean(x, 0)  # [1.5, 1.5]
    tf.reduce_mean(x, 1)  # [1.,  2.]
```
```buildoutcfg
    x = np.array([ [ [1,2], [2,3], [3,4] ], [ [2,3], [1,3], [4,2] ] ]).astype('float')
    tf.reduce_mean(x, 0) 
    # array([[ 1.5,  2.5], [ 1.5,  3. ], [ 3.5,  3. ]])
    tf.reduce_mean(x, 1) 
    # array([[ 2. ,  3. ], [ 2.33333325,  2.66666675]])
    tf.reduce_mean(x, 2) 
    # array([[ 1.5,  2.5,  3.5], [ 2.5,  2. ,  3. ]])

```

- [__`tf.reduce_sum`__](https://www.tensorflow.org/api_docs/python/tf/reduce_sum)
```buildoutcfg
    x = tf.constant([[1, 1, 1], [1, 1, 1]])
    tf.reduce_sum(x)  # 6
    tf.reduce_sum(x, 0)  # [2, 2, 2]
    tf.reduce_sum(x, 1)  # [3, 3]
    tf.reduce_sum(x, 1, keepdims=True)  # [[3], [3]]
    tf.reduce_sum(x, [0, 1])  # 6
```

- [__`tf.reshape`__](https://www.tensorflow.org/api_docs/python/tf/reshape)
MNIST data input is a 1-D vector of 784 features (28*28 pixels). 
Reshape to match picture format [Height x Width x Channel]. Tensor input become 4-D: [Batch Size, Height, Width, Channel]

```
    x = x_dict['images']
    x = tf.reshape(x, shape=[-1, 28, 28, 1])
```
Here "-1" is inferred to be the batch size. 
```
    reshape(x, [-1]) ==> flatten x
```
#### 2. Neural Network

- [__`tf.layers.conv2d`__]() Convolution Layer with 32 filters and a kernel size of 5
```
    conv1 = tf.layers.conv2d(x, 32, 5, activation=tf.nn.relu)
```
- [__`tf.layers.max_pooling2d`__]() Max Pooling (down-sampling) with strides of 2 and kernel size of 2
```
    conv1 = tf.layers.max_pooling2d(conv1, 2, 2)
```
- [__`tf.contrib.layers.flatten`__]() Flatten the data to a 1-D vector for the fully connected layer
```
    fc1 = tf.contrib.layers.flatten(conv2)
```
- [__`tf.layers.dense`__]() Fully connected layer (in tf contrib folder for now)
```
    fc1 = tf.layers.dense(fc1, 1024)
```

###### 2.2 RNN

- [__`LSTMStateTuple`__](https://stackoverflow.com/questions/43192809/lstmstatetuple-vs-cell-zero-state-for-rnn-in-tensorflow) vs [__`cell.zero_state`__](https://stackoverflow.com/questions/43192809/lstmstatetuple-vs-cell-zero-state-for-rnn-in-tensorflow)
The two are different things. `state_is_tuple` is used on LSTM cells because the state of LSTM cells is a tuple. 
`cell.zero_state` is the initializer of the state for all RNN cells.
    1. You will generally prefer `cell.zero_state` function as it will initialize the required state class depending on 
    whether state_is_tuple is true or not.
    2. You'll want to use `LSTMStateTuple` when you're initializing your state with custom values (passed by the trainer). 
    `cell.zero_state()` will return the state with all the values equal to 0.0.
    3. If you want to keep state between batches than you'll have to get it after each batch and add it to your 
    feed_dict the next batch.

