

some error fix

2020-04-05 13:42:11.293135: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudnn.so.7
2020-04-05 13:42:11.620920: E tensorflow/stream_executor/cuda/cuda_dnn.cc:329] Could not create cudnn handle: CUDNN_STATUS_INTERNAL_ERROR
2020-04-05 13:42:11.633906: E tensorflow/stream_executor/cuda/cuda_dnn.cc:329] Could not create cudnn handle: CUDNN_STATUS_INTERNAL_ERROR


import tensorflow as tf
gpu_devices = tf.config.experimental.list_physical_devices('GPU')
for device in gpu_devices: tf.config.experimental.set_memory_growth(device, True)



config = tf.ConfigProto(log_device_placement=True)
config.gpu_options.per_process_gpu_memory_fraction=0.3 # don't hog all vRAM
config.operation_timeout_in_ms=15000   # terminate on long hangs
sess = tf.InteractiveSession("", config=config)



According to TF 1:1 Symbols Map, in TF 2.0 you should use tf.compat.v1.Session() instead of tf.Session()

https://docs.google.com/spreadsheets/d/1FLFJLzg7WNP6JHODX5q8BDgptKafq_slHpnHVbJIteQ/edit#gid=0

To get TF 1.x like behaviour in TF 2.0 one can run

import tensorflow.compat.v1 as tf
tf.disable_v2_behavior()

but then one cannot benefit of many improvements made in TF 2.0. For more details please refer to the migration guide https://www.tensorflow.org/guide/migrate

import os
os.environ["CUDA_VISIBLE_DEVICES"] = "-1"



gpu_devices = tf.config.experimental.list_physical_devices('GPU')
for device in gpu_devices: tf.config.experimental.set_memory_growth(device, True)
#gpu_devices = tf.config.list_physical_devices('GPU')
#for device in gpu_devices: tf.config.set_memory_growth(device, True)

#physical_devices = tf.config.list_physical_devices('GPU') 
#try:    
  # Disable all GPUS 
#  tf.config.set_visible_devices([], 'GPU') 
#  visible_devices = tf.config.get_visible_devices() 
#  for device in visible_devices: 
  #  assert device.device_type != 'GPU' 
#     print(device)
#except: 
#  print ("invalid device initization")
#  pass 
