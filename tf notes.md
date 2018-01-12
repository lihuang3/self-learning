## Tensorflow Learning Notebook

#### 2. Copy Parameters
- Copy weights from nn2 to nn1 (version 1)

Inside each neural network class, get the list of trainable variables
```
    nn1.var_list = tf.get_collection(tf.GraphKeys.TRAINABLE_VARIABLES, tf.get_variable_scope().name)
```
Then
```    
    sync_var_list = [v1.assign(v2) for v1, v2 in zip(nn1.var_list, nn2.varlist)]
    sync_op = tf.group(*sync_var_list)
    sess.run(sync_op)
```


- Copy weights from nn2 to nn1 (version 2)
```
def make_copy_params_op(v1_list, v2_list):
  
  # Creates an operation that copies parameters from variable in v1_list to variables in v2_list.
  # The ordering of the variables in the lists must be identical.
 
  v1_list = list(sorted(v1_list, key=lambda v: v.name))
  v2_list = list(sorted(v2_list, key=lambda v: v.name))
  
  update_ops = []
  for v1, v2 in zip(v1_list, v2_list):
    op = v2.assign(v1)
    update_ops.append(op)
    
  return update_ops
  
sync_op =  make_copy_params_op(
      tf.contrib.slim.get_variables(scope="nn1", collection=tf.GraphKeys.TRAINABLE_VARIABLES),
      tf.contrib.slim.get_variables(scope="nn2", collection=tf.GraphKeys.TRAINABLE_VARIABLES))

```

#### 1. Save and restore checkpoints
- Create an experiment path to save checkpoints and saummries:

```
    experiment_dir = os.path.abspath('./experiments/')
```

- Create directories for the experiments and checkpoints
```checkpoint_dir = os.path.join(experiment_dir, 'checkpoint')
    if not os.path.exists(checkpoint_dir):
        os.makedirs(checkpoint_dir)
    checkpoint_path = os.path.join(checkpoint_dir, 'model')
```
- Create a saver object
```
    saver = tf.train.Saver()
```
- Load the latest checkpoint
```    
    latest_checkpoint = tf.train.latest_checkpoint(checkpoint_dir)
    if latest_checkpoint:
        print('Loading the latest checkpoint from ... \n {}'.format(latest_checkpoint))
        saver.restore(sess, latest_checkpoint)
```
- Save the checkpoint to the path
```
    init_op = tf.global_variables_initializer()
    with tf.Session() as sess:
    sess.run(init_op)
    saver.save(sess, checkpoint_path)
```
[Ref](): [1]Tensorflow official tutorial 
https://www.tensorflow.org/programmers_guide/saved_model

