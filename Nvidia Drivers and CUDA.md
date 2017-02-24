#Install Nvidia driver on Ubuntu 16.04
安裝 nvidia driver 必須注意 cuda 支援版本, 至少 **higher/newer**

**直接裝 CUDA 話，也會包含 Driver 一起安裝, 可直接略過此步驟，但版本未必是最新**
```
CUDA 8.0: 367.xx  
CUDA 7.5: 352.xx
CUDA 7.0: 346.xx
CUDA 6.5: 340.xx
CUDA 6.0: 331.xx
CUDA 5.5: 319.xx
CUDA 5.0: 304.xx
CUDA 4.2: 295.41
CUDA 4.1: 285.05.33
CUDA 4.0: 270.41.19
CUDA 3.2: 260.19.26
CUDA 3.1: 256.40
CUDA 3.0: 195.36.15
```
軟體目錄下新增 ppa 地址，然後更新資料庫資訊

```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
```

可以由此 [Proprietary GPU Drivers](https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa) 查看目前資料庫的版本

#### 安裝 dirver 375版
安裝前必須先關閉桌面環境 **lightdm**

```
sudo service lightdm stop
sudo apt-get install nvidia-375 nvidia-settings
```

重開機, 輸入 `nvidia-smi`, 安裝成功會顯示顯卡資訊

查看 driver 版本:

`cat /proc/driver/nvidia/version`

##CUDA
首先必須要為 Nvidia 的顯示卡才能做 CUDA 運算, 底下由 CUDA8.0 作為範例


### 下載 CUDA8.0 安裝檔：
[Nvidia CUDA](https://developer.nvidia.com/cuda-downloads) 選擇 `CUDA8.0.runfile (local)`


## Install CUDA8.0

安裝前必須先關閉桌面環境 **lightdm**

`sudo service lightdm stop`

接著安裝 CUDA

`sudo sh cuda_8.0.44_linux.run`

若已經安裝 Driver，可以略過安裝 Driver 這一步驟

步驟中有選擇安裝 CUDA Sample，可以選擇安裝在哪, 或在網路上另外抓
##設定PATH  

將以下加入 `~/.bashrc` ：
(Global setting, 設定在 `/etc/profile` 下)
```
export CUDA_HOME=/usr/local/cuda
export CUDA_ROOT=$CUDA_HOME
export CUDA_INC_DIR=$CUDA_HOME/include/
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64
PATH=${CUDA_HOME}/bin:${PATH}
export PATH
```
設定完後，記得 ` source ~/.bashrc`
可以透過 `nvcc --version` 查看 cuda 是否安裝成功

```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2016 NVIDIA Corporation
Built on Sun_Sep__4_22:14:01_CDT_2016
Cuda compilation tools, release 8.0, V8.0.44
```
##CUDA Samples
安裝 CUDA Samples 目的主要是驗證 CUDA 安裝是否成功
如果編譯 CUDA Samples 沒有 Errors (Warning 忽略), 就表示沒問題

```
# 切換到 NVIDIA CUDA-8.0 Samples
cd /usr/local/cuda/samples


# 編譯 CUDA Sample
make –j8

# 編譯完後，切換到 release 目錄
cd /usr/local/cuda/samples/bin/x86_64/linux/release

# 驗證是否成功, 會顯示 NVIDIA 顯卡資訊
./deviceQuery 
```
應該會出現這樣的訊息

```
CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "GeForce GTX TITAN"
  CUDA Driver Version / Runtime Version          8.0 / 8.0
  CUDA Capability Major/Minor version number:    3.5
  Total amount of global memory:                 6109 MBytes (6405685248 bytes)
  (14) Multiprocessors, (192) CUDA Cores/MP:     2688 CUDA Cores
  GPU Max Clock rate:                            928 MHz (0.93 GHz)
  Memory Clock rate:                             3004 Mhz
  Memory Bus Width:                              384-bit
  L2 Cache Size:                                 1572864 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(65536), 2D=(65536, 65536), 3D=(4096, 4096, 4096)
  Maximum Layered 1D Texture Size, (num) layers  1D=(16384), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(16384, 16384), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 1 copy engine(s)
  Run time limit on kernels:                     No
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 8.0, CUDA Runtime Version = 8.0, NumDevs = 1, Device0 = GeForce GTX TITAN
Result = PASS

```
## ldconfig

進入 `/etc/ld.so.conf.d ` 下, 建立新文件 `cuda.conf`：
```
/usr/local/cuda/lib64
/lib
/usr/lib
/usr/lib32
```
完成後 `sudo ldconfig -v` 使其生效

## Install cuDNN
cuDNN 是為了 Deep Neural Network 設計的 GPU 加速函式庫
首先先到 [cuDNN Offical](https://developer.nvidia.com/cudnn) 依照 CUDA 版本下載， 順便下載 Sample Code 做為測試，這邊使用 cuDNN 5.1

解壓縮檔案 :`tar xvzf cudnn-8.0-linux-x64-v5.1.solitairetheme8`

會產生出一個 CUDA 的資料夾, 進入資料夾後
```
sudo cp -P include/cudnn.h /usr/local/cuda/include
sudo cp -P lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

###確認複製

`cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`

輸入後出現大概如此
```
#define CUDNN_MAJOR      5
#define CUDNN_MINOR      1
#define CUDNN_PATCHLEVEL 5
--
#define CUDNN_VERSION    (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#include "driver_types.h"

```

### 測試 cuDNN 
從下載的 Sample Code 做為測試, 一樣先解壓縮, 這邊使用之前 cuDNN5 的 sample code

`tar xvzf cudnn-sample-v5.solitairetheme8`

切換到解壓縮後的目錄，拿 mnistCUDNN 做測試
```
make 
./mnistCUDNN
```

跑出一大堆訊息
```
cudnnGetVersion() : 5105 , CUDNN_VERSION from cudnn.h : 5105 (5.1.5)
Host compiler version : GCC 5.4.0
There are 1 CUDA capable devices on your machine :
device 0 : sms 14  Capabilities 3.5, SmClock 928.0 Mhz, MemSize (Mb) 6108, MemClock 3004.0 Mhz, Ecc=0, boardGroupID=0
Using device 0

Testing single precision
Loading image data/one_28x28.pgm
Performing forward propagation ...
Testing cudnnGetConvolutionForwardAlgorithm ...
Fastest algorithm is Algo 0
Testing cudnnFindConvolutionForwardAlgorithm ...
^^^^ CUDNN_STATUS_SUCCESS for Algo 0: 0.025856 time requiring 0 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 1: 0.036128 time requiring 100 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 2: 0.046560 time requiring 57600 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 4: 0.138144 time requiring 207360 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 5: 0.142048 time requiring 205008 memory
Resulting weights from Softmax:
0.0000000 0.9999399 0.0000000 0.0000000 0.0000561 0.0000000 0.0000012 0.0000017 0.0000010 0.0000000
Loading image data/three_28x28.pgm
Performing forward propagation ...
Resulting weights from Softmax:
0.0000000 0.0000000 0.0000000 0.9999288 0.0000000 0.0000711 0.0000000 0.0000000 0.0000000 0.0000000
Loading image data/five_28x28.pgm
Performing forward propagation ...
Resulting weights from Softmax:
0.0000000 0.0000008 0.0000000 0.0000002 0.0000000 0.9999820 0.0000154 0.0000000 0.0000012 0.0000006

Result of classification: 1 3 5

Test passed!

Testing half precision (math in single precision)
Loading image data/one_28x28.pgm
Performing forward propagation ...
Testing cudnnGetConvolutionForwardAlgorithm ...
Fastest algorithm is Algo 0
Testing cudnnFindConvolutionForwardAlgorithm ...
^^^^ CUDNN_STATUS_SUCCESS for Algo 0: 0.028192 time requiring 0 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 1: 0.036352 time requiring 100 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 2: 0.046720 time requiring 28800 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 5: 0.143520 time requiring 205008 memory
^^^^ CUDNN_STATUS_SUCCESS for Algo 4: 0.144288 time requiring 207360 memory
Resulting weights from Softmax:
0.0000001 1.0000000 0.0000001 0.0000000 0.0000563 0.0000001 0.0000012 0.0000017 0.0000010 0.0000001
Loading image data/three_28x28.pgm
Performing forward propagation ...
Resulting weights from Softmax:
0.0000000 0.0000000 0.0000000 1.0000000 0.0000000 0.0000714 0.0000000 0.0000000 0.0000000 0.0000000
Loading image data/five_28x28.pgm
Performing forward propagation ...
Resulting weights from Softmax:
0.0000000 0.0000008 0.0000000 0.0000002 0.0000000 1.0000000 0.0000154 0.0000000 0.0000012 0.0000006

Result of classification: 1 3 5

Test passed!
```

有出現 `Test Passed` ,就代表成功安裝
## Install CNMem 
先從 Nvidia Github 下載

`git clone https://github.com/NVIDIA/cnmem.git cnmem`

**Build CNMeM with the unit tests**
```
cd cnmem
mkdir build
cd build
cmake -DWITH_TESTS=True ..
make
```

**補充說明**
上述設定 `CUDA_HOME` 路徑是 `/usr/local/cuda`
其實 `/usr/local/cuda` 其實就是安裝路徑的 `/usr/local/cuda-7.5` 同步連結
在 `/usr/local/` 下查看詳細資訊, 會看到

```
lrwxrwxrwx  1 root root    8  6月 30 13:51 cuda -> cuda-7.5/
```

加入 `ld.so.conf` 目的為將動態函式庫載入快取記憶體
可以使用 `sudo ldconfig -p ` 查看有戴入的動態函示庫清單


## 後記

使用 jupyter notebook import tensorflow 會發生以下情況

`Couldn't open CUDA library libcudnn.so. LD_LIBRARY_PATH:`

根據 Google 大神的查詢下找到一個用 symlinks [解法](https://devtalk.nvidia.com/default/topic/936212/tensorflow-cannot-find-cudnn-ubuntu-16-04-cuda7-5-/) 


```
# Put symlinks in /usr/local/cuda
cd /usr/local/cuda
sudo ln -s  /usr/lib/x86_64-linux-gnu/ lib64
sudo ln -s  /usr/include/ include
sudo ln -s  /usr/bin/ bin
sudo ln -s  /usr/lib/x86_64-linux-gnu/ nvvm
sudo mkdir -p extras/CUPTI
cd extras/CUPTI
sudo ln -s  /usr/lib/x86_64-linux-gnu/ lib64
sudo ln -s  /usr/include/ include

# Install cuDnn
#http://askubuntu.com/questions/767269/how-can-i-install-cudnn-on-ubuntu-16-04
# Download cudann as detailed above and extract
cd ~/Downloads/cuda
sudo cp include/cudnn.h /usr/include
sudo cp lib64/libcudnn* /usr/lib/x86_64-linux-gnu/
sudo chmod a+r /usr/lib/x86_64-linux-gnu/libcudnn*
```

