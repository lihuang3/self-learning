- [`tf.reshape`]: MNIST data input is a 1-D vector of 784 features (28*28 pixels). Reshape to match picture format [Height x Width x Channel]. Tensor input become 4-D: [Batch Size, Height, Width, Channel]
```
    x = x_dict['images']
    x = tf.reshape(x, shape=[-1, 28, 28, 1])
```
