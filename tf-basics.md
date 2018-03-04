- [`tf.reshape`]: MNIST data input is a 1-D vector of 784 features (28*28 pixels). Reshape to match picture format [Height x Width x Channel]. Tensor input become 4-D: [Batch Size, Height, Width, Channel]

```
 x = x_dict['images']
 x = tf.reshape(x, shape=[-1, 28, 28, 1])
```
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
