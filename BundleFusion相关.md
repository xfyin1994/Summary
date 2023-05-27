# BundleFusion

# Windows kinect One(V2)配置
### 基本环境要求及参考
VS2013 + Cuda 8.0/Cuda10.0[下载地址](https://developer.nvidia.com/cuda-toolkit-archive) + OpenCV3.0.0 

[初始配置参考文章](https://zhoujie1994.cn/2019/04/12/21/)

### 连接Kinect one 出现卡住的情况

**1. 需要对 `DepthSensing.cpp` 做如下修改：**
```
# 将986行
bGotDepth = g_CudaImageManager->process()
# 替换为
bool bGotDepth;
while (!(bGotDepth = g_CudaImageManager->process()));
```

[参考github issue的回答进行线程相关修改，具体回答见下文](https://github.com/niessner/BundleFusion/issues/6#issuecomment-468109093)

i have a solutions that works for me with RUN_MULTITHREADED
In `DepthSensing.cpp` line 987 replace
`bGotDepth = g_CudaImageManager->process()`
with
`bool bGotDepth; while (!(bGotDepth = g_CudaImageManager->process()));`

The problem that i investigated was that the kinect one provides 30fps max. If the frame is not ready to fetch, pDepthFrameReference->AcquireFrame returns a FAIL. This causes a dead lock in the process lock management. The while loops forces the CALLBACK to wait untill a new frame is available. Hope this helps.

**2. 升级Cuda到10.0，同时更改部分代码**

2.1 [升级cuda到10.0](https://github.com/niessner/BundleFusion/issues/6#issuecomment-706874803)

2.2 [修改`ProgramCU.cu`文件中的对应代码（查看Commits中的修改）,具体内容如下：](https://github.com/niessner/BundleFusion/pull/56#issue-425077863)

```
@@ -989,7 +989,7 @@ void __global__ ComputeOrientation_Kernel(float4* d_list,
			const unsigned int p = (tidx + 1) % 36;
			target[tidx] = (source[m] + source[c] + source[p])*one_third;

			__syncthreads();  //- 注释或删除次行代码
			__threadfence();  //+ 添加此行代码
			volatile float *tmp = source;
			source = target;
			target = tmp;
@@ -1100,8 +1100,8 @@ void __global__ ComputeOrientation_Kernel(float4* d_list,
	__syncthreads();
	if (tidx == 0) {
		weights[maxIndex] = -1.0f;
		__syncthreads();  //- 注释或删除次行
	}
	__syncthreads();  //+ 添加此行代码

	// 2nd reduction to compute 2nd max weight
	for (unsigned int stride = COMPUTE_ORIENTATION_BLOCK / 2; stride > 0; stride /= 2) {
```
