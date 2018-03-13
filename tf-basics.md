### Basic tensorflow functions and commands

#### 1. Array operations


- [`tf.to_float()`]()

- [`tf.shape()`](https://www.tensorflow.org/api_docs/python/tf/shape) this is the op to get the shape of a tensor.

- [`tf.reduce_mean(input_tensor, axis)`](https://www.tensorflow.org/api_docs/python/tf/reduce_mean):axis ranges from -rank(input_tensor) to rank(input_tensor). 
__e.g.__ axis = 0, 1, 2 correspond to outer most bracket dimension, the second outer most bracket dimension, etc.  
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

- [`tf.reduce_sum`](https://www.tensorflow.org/api_docs/python/tf/reduce_sum):__e.g.__
```buildoutcfg
    x = tf.constant([[1, 1, 1], [1, 1, 1]])
    tf.reduce_sum(x)  # 6
    tf.reduce_sum(x, 0)  # [2, 2, 2]
    tf.reduce_sum(x, 1)  # [3, 3]
    tf.reduce_sum(x, 1, keepdims=True)  # [[3], [3]]
    tf.reduce_sum(x, [0, 1])  # 6
```

- [`tf.reshape`](https://www.tensorflow.org/api_docs/python/tf/reshape):__e.g.__ 
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

- [`tf.layers.conv2d`]: Convolution Layer with 32 filters and a kernel size of 5
```
    conv1 = tf.layers.conv2d(x, 32, 5, activation=tf.nn.relu)
```
- [`tf.layers.max_pooling2d`]: Max Pooling (down-sampling) with strides of 2 and kernel size of 2
```
    conv1 = tf.layers.max_pooling2d(conv1, 2, 2)
```
- [`tf.contrib.layers.flatten`]: Flatten the data to a 1-D vector for the fully connected layer
```
    fc1 = tf.contrib.layers.flatten(conv2)
```
- [tf.layers.dense]: Fully connected layer (in tf contrib folder for now)
```
    fc1 = tf.layers.dense(fc1, 1024)
```
