#Theano
[Theano 官方安裝過程](http://deeplearning.net/software/theano/install.html)
[Thano Github](https://github.com/Theano)
此篇會對Python的管理套件一併安裝說明。

##Install pip
pip 是一套Python套件管理程式
安裝之前可能先需要安裝一些 denpencey :
######pythn 2.x
`sudo apt-get install python-setuptools python-dev`
######python 3.x 
`sudo apt-get install python3-setuptools python3-dev`

主要有兩種方法，比較推薦使用第一種方式安裝
1. ubuntu apt-get 方式
```
sudo apt-get install python3-pip	 #Python 3.x
sudo apt-get install python-pip 	 #Python 2.x
```
2. 下載 [get-pip.py](https://bootstrap.pypa.io/get-pip.py)，自行用 python 編譯，會一起安裝pip跟pip3
`sudo python get-pip.py`

##前置設定

安裝numpy/scipy/theano 之前必須先安裝以下套件

`sudo apt-get install build-essential libboost-python-dev libboost-thread-dev `

`sudo apt-get install libboost-all-dev # Boost C++ Libraries`

`sudo sudo apt-get install gcc `

`sudo apt-get install g++`

`sudo apt-get install git`

`sudo apt-get install gfortran 	# 安裝gfortran,後面編譯過程中會用到`

`sudo apt-get install libopenblas-dev	 # 安裝 openblas, theano 指定使用這版本`
`sudo apt-get install liblapack-dev`

安裝完 openblas ,可以查看一下目前系統使用的 blas lib 狀況

`update-alternatives --get-selections | grep libblas`

應該會看到下列情況
```
libblas.so                     auto     /usr/lib/openblas-base/libblas.so
libblas.so.3                   auto     /usr/lib/openblas base/libblas.so.3
```
**以下安裝皆以 python3 為主**
**若使用 python2 則使用 pip**
##Install nose
Nose為Python中的 Test framework

`sudo pip3 install nose`

另外還需要安裝 `nose-parameterized`

`sudo pip3 install nose-parameterized`
##Install numpy 
`sudo pip3 install numpy`

測試numpy是否正常，在python的terminal下

`import numpy`
`numpy.test()`



要確認error要為 0，表示正確安裝。

`nose.result.TextTestResult run=XXX errors=0 failures=0`

##Install scipy#
`sudo pip3 install scipy`

測試scipy是否正常，進python的terminal下

`import scipy`
`scipy.test()`

要確認error要為 0，表示正確安裝

`nose.result.TextTestResult run=XXX errors=0 failures=0`

##Install theano
`sudo pip3 install Theano`

若要指定版本話

`sudo pip3 install -v theano==0.8.0`

測試theano是否正常，進python的terminal下

`import theano`
`theano.test()`

要確認error要為 0，表示正確安裝。

`nose.result.TextTestResult run=XXX errors=0 failures=0`

若有出現error，可以先更新theano版本看看

`sudo pip3 install --upgrade --no-deps git+git://github.com/Theano/Theano.git` 


#### Install PyCUDA
**安裝之前需要先安裝好 Nvidia drivers 和 CUDA**

Python 上使用 cuda，需要安裝 PyCUDA
```
sudo su
pip install pycuda
```
**測試 PyCUDA**
```
import pycuda.autoinit
import pycuda.driver as cuda
import pprint
device = cuda.Device(0)

pp = pprint.PrettyPrinter(depth=10)
pp.pprint(device.get_attributes())
```
會得到類似下列輸出結果
```
{pycuda._driver.device_attribute.MAX_THREADS_PER_BLOCK: 1024,
 pycuda._driver.device_attribute.MAX_BLOCK_DIM_X: 1024,
 pycuda._driver.device_attribute.MAX_BLOCK_DIM_Y: 1024,
 pycuda._driver.device_attribute.MAX_BLOCK_DIM_Z: 64,
 pycuda._driver.device_attribute.MAX_GRID_DIM_X: 65535,
 pycuda._driver.device_attribute.MAX_GRID_DIM_Y: 65535,
 pycuda._driver.device_attribute.MAX_GRID_DIM_Z: 65535,
 pycuda._driver.device_attribute.MAX_SHARED_MEMORY_PER_BLOCK: 49152,
 pycuda._driver.device_attribute.TOTAL_CONSTANT_MEMORY: 65536,
 pycuda._driver.device_attribute.WARP_SIZE: 32,
 pycuda._driver.device_attribute.MAX_PITCH: 2147483647,
 pycuda._driver.device_attribute.MAX_REGISTERS_PER_BLOCK: 32768,
 pycuda._driver.device_attribute.CLOCK_RATE: 1700000,
 pycuda._driver.device_attribute.TEXTURE_ALIGNMENT: 512,
 pycuda._driver.device_attribute.GPU_OVERLAP: 1,
 pycuda._driver.device_attribute.MULTIPROCESSOR_COUNT: 7,
 pycuda._driver.device_attribute.KERNEL_EXEC_TIMEOUT: 1,
 pycuda._driver.device_attribute.INTEGRATED: 0,
 pycuda._driver.device_attribute.CAN_MAP_HOST_MEMORY: 1,
 pycuda._driver.device_attribute.COMPUTE_MODE: pycuda._driver.compute_mode.DEFAULT,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE1D_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_HEIGHT: 65535,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE3D_WIDTH: 2048,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE3D_HEIGHT: 2048,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE3D_DEPTH: 2048,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_ARRAY_WIDTH: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_ARRAY_HEIGHT: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_ARRAY_NUMSLICES: 2048,
 pycuda._driver.device_attribute.SURFACE_ALIGNMENT: 512,
 pycuda._driver.device_attribute.CONCURRENT_KERNELS: 1,
 pycuda._driver.device_attribute.ECC_ENABLED: 0,
 pycuda._driver.device_attribute.PCI_BUS_ID: 1,
 pycuda._driver.device_attribute.PCI_DEVICE_ID: 0,
 pycuda._driver.device_attribute.TCC_DRIVER: 0,
 pycuda._driver.device_attribute.MEMORY_CLOCK_RATE: 2100000,
 pycuda._driver.device_attribute.GLOBAL_MEMORY_BUS_WIDTH: 256,
 pycuda._driver.device_attribute.L2_CACHE_SIZE: 524288,
 pycuda._driver.device_attribute.MAX_THREADS_PER_MULTIPROCESSOR: 1536,
 pycuda._driver.device_attribute.ASYNC_ENGINE_COUNT: 1,
 pycuda._driver.device_attribute.UNIFIED_ADDRESSING: 1,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE1D_LAYERED_WIDTH: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE1D_LAYERED_LAYERS: 2048,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_GATHER_WIDTH: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_GATHER_HEIGHT: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE3D_WIDTH_ALTERNATE: 0,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE3D_HEIGHT_ALTERNATE: 0,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE3D_DEPTH_ALTERNATE: 0,
 pycuda._driver.device_attribute.PCI_DOMAIN_ID: 0,
 pycuda._driver.device_attribute.TEXTURE_PITCH_ALIGNMENT: 32,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURECUBEMAP_WIDTH: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURECUBEMAP_LAYERED_WIDTH: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURECUBEMAP_LAYERED_LAYERS: 2046,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE1D_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE2D_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE2D_HEIGHT: 32768,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE3D_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE3D_HEIGHT: 32768,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE3D_DEPTH: 2048,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE1D_LAYERED_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE1D_LAYERED_LAYERS: 2048,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE2D_LAYERED_WIDTH: 65536,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE2D_LAYERED_HEIGHT: 32768,
 pycuda._driver.device_attribute.MAXIMUM_SURFACE2D_LAYERED_LAYERS: 2048,
 pycuda._driver.device_attribute.MAXIMUM_SURFACECUBEMAP_WIDTH: 32768,
 pycuda._driver.device_attribute.MAXIMUM_SURFACECUBEMAP_LAYERED_WIDTH: 32768,
 pycuda._driver.device_attribute.MAXIMUM_SURFACECUBEMAP_LAYERED_LAYERS: 2046,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE1D_LINEAR_WIDTH: 134217728,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_LINEAR_WIDTH: 65000,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_LINEAR_HEIGHT: 65000,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_LINEAR_PITCH: 1048544,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_MIPMAPPED_WIDTH: 16384,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE2D_MIPMAPPED_HEIGHT: 16384,
 pycuda._driver.device_attribute.COMPUTE_CAPABILITY_MAJOR: 2,
 pycuda._driver.device_attribute.COMPUTE_CAPABILITY_MINOR: 1,
 pycuda._driver.device_attribute.MAXIMUM_TEXTURE1D_MIPMAPPED_WIDTH: 16384,
 pycuda._driver.device_attribute.STREAM_PRIORITIES_SUPPORTED: 0,
 pycuda._driver.device_attribute.GLOBAL_L1_CACHE_SUPPORTED: 1,
 pycuda._driver.device_attribute.LOCAL_L1_CACHE_SUPPORTED: 1,
 pycuda._driver.device_attribute.MAX_SHARED_MEMORY_PER_MULTIPROCESSOR: 49152,
 pycuda._driver.device_attribute.MAX_REGISTERS_PER_MULTIPROCESSOR: 32768,
 pycuda._driver.device_attribute.MANAGED_MEMORY: 0,
 pycuda._driver.device_attribute.MULTI_GPU_BOARD: 0,
 pycuda._driver.device_attribute.MULTI_GPU_BOARD_GROUP_ID: 1}

```
#####CNMeM
CNMeM 主要是管理 CUDA 在做 deep learning 記憶體的 library
先從官方 github download 下來

`git clone https://github.com/NVIDIA/cnmem.git cnmem`

編譯 cnmem，在複製到 cuda7.5 目錄
```
cd cnmem
mkdir build
cd build
cmake ..
make
sudo cp include/cnmem.h /usr/local/cuda-7.5/include
sudo cp build/*.so  /usr/local/cuda-7.5/lib64/
```
若設定後無法啟動   
可能安裝 CUDA 版本問題 (CUDA >= 7.0) 或 Cuda 沒有 CNMeM header file  

#####cuDNN
先到 [Nvidia cuDNN](https://developer.nvidia.com/cudnn)下載，這邊我們使用的是 cuda-7.5
下載完後解壓縮：

`tar xvzf cudnn-7.5-linux-x64-v5.0-ga.tgz`

解壓縮後會出現一個文件夾 `cuda`，接著將所需的 `cuDNN` 檔案複製到 cuda安裝位置

```
sudo cp cuda/include/cudnn.h /usr/local/cuda-7.5/include/
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-7.5/lib64/
sudo chmod a+r /usr/local/cuda-7.5/lib64/libcudnn*
```

##Theano GPU setting
官方上的 [Config](http://deeplearning.net/software/theano/library/config.html) 設定說明  

可以先透過 `python -c 'import theano; print(theano.config)' | less` 取得目前 theano configuration variables     

一般設定 theano 有三種方式
1. 定義 $CUDA_ROOT 環境變數 e.g. CUDA_ROOT=/usr/local/cuda/
2. 在 THEANO_FLAGS 新增 cuda.root 標籤, 每當 run theano 時則會輸入：  
`THEANO_FLAGS='cuda.root=/usr/local/cuda/' python <myscript>.py`
3. 修改　.theanorc 設定

以上方法最推薦, **第三種**作法，如果 server 上有多種程式需要用到 cuda 話
第一種作法可能造成 bashrc 凌亂，或彼此設定可能造成影響
第二種則每次執行很麻煩
##### 設定 theanorc
一般 `.theanorc` 位置在 `~/.theanorc`，若沒有話自行創建
新增內容：
```
[global]
floatX = float32 # 設定預設的浮點計算類型(float32)
device = gpu # 使用裝置gpu, 也可以指定 gpu0 以此類推

[nvcc]
fastmath = True # 設定 nvcc
 
[cuda]
root = /usr/local/cuda-7.0/ #設定 cuda 位置

[lib]
cnmem = 1 # 設定 Theano 可以使用 gpu 上記憶體比例， 設定值為 0~1
```


######config 解釋
首先要了解 `.theanorc` 上 section
theano 在設定上會有定義 Config Attributes
device 是 `config.device`， 所在的 section 為 `[global]`
cnmem 是 `config.lib.cnmem`， 所在 section 為 `[lib]`
algo_fwd 是 `config.dnn.conv.algo_fwd `， 所在 section 為 `[dnn.conv]`

更改裝置也可以使用上述第二種方式

`THEANO_FLAGS=’cuda.root=/usr/local/cuda/bin/,device=gpu,floatX=float32’`

以上設定完後可以使用 Theano 官方提供 [source code](http://deeplearning.net/software/theano/tutorial/using_gpu.html) 做測試，執行結果會告知是使用 CPU or GPU 做計算
