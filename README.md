# Summary

## Tensorflow多GPU多进程占用的问题
Tensorflow默认会使用尽可能多的GPU，并且占用所使用的GPU。因此如果有别的Tensorflow正在使用GPU，而自己的程序使用默认配置，那么是无法使用已经被使用的GPU的，也无法单独使用一块没有被使用的GPU。 
因此，我们可以在运行我们的tensorflow程序的时候，指定程序使用的特定GPU：

### Ⅰ. 在终端上配置：
```CUDA_VISIBLE_DEVICES=0 python your_python.py```
或者：
```
export CUDA_VISIBLE_DEVICES="0"
python your_python.py
```
当然，你要先看看你的服务器的GPU配置，可以使用nvidia-smi命令,然后也可以指定多个GPU。

### Ⅱ. 在代码中配置：
```
import os
os.environ["CUDA_VISIBLE_DEVICES"]="0"
```
另外，我们也可以指定我们使用的显存比例：
```
config = tf.ConfigProto()
config.gpu_options.per_process_gpu_memory_fraction = 0.4
session = tf.Session(config=config, ...)
```
