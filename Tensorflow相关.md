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
或者
```
gpu_fraction = 0.1
gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=gpu_fraction)
sess = tf.Session(config=tf.ConfigProto(gpu_options=gpu_options))
```

## tensorflow中使用指定的GPU及GPU显存

### 1 终端执行程序时设置使用的GPU
如果电脑有多个GPU，tensorflow默认全部使用。如果想只使用部分GPU，可以设置CUDA_VISIBLE_DEVICES。在调用python程序时，可以使用（见第一个参考网址Franck Dernoncourt的回复）：

CUDA_VISIBLE_DEVICES=1 python my_script.py
 

```
Environment Variable Syntax      Results

CUDA_VISIBLE_DEVICES=1           Only device 1 will be seen
CUDA_VISIBLE_DEVICES=0,1         Devices 0 and 1 will be visible
CUDA_VISIBLE_DEVICES="0,1"       Same as above, quotation marks are optional
CUDA_VISIBLE_DEVICES=0,2,3       Devices 0, 2, 3 will be visible; device 1 is masked
CUDA_VISIBLE_DEVICES=""          No GPU will be visible
```
### 2 python代码中设置使用的GPU
如果要在python代码中设置使用的GPU（如使用pycharm进行调试时），可以使用下面的代码（见第二个参考网址中Yaroslav Bulatov的回复）：
```
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "2"
```
### 3 设置tensorflow使用的显存大小
#### 3.1 定量设置显存
默认tensorflow是使用GPU尽可能多的显存。可以通过下面的方式，来设置使用的GPU显存：
```
gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=0.7)
sess = tf.Session(config=tf.ConfigProto(gpu_options=gpu_options))        
```
上面分配给tensorflow的GPU显存大小为：GPU实际显存*0.7。

可以按照需要，设置不同的值，来分配显存。

#### 3.2 按需设置显存
上面的只能设置固定的大小。如果想按需分配，可以使用allow_growth参数（参考网址：http://blog.csdn.net/cq361106306/article/details/52950081）：
```
gpu_options = tf.GPUOptions(allow_growth=True)
sess = tf.Session(config=tf.ConfigProto(gpu_options=gpu_options))   
```


