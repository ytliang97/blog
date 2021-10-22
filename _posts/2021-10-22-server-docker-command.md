---
layout: post
comments: true
title: 機器學習遠端server docker常見操作指令 | Docker jupyter
excerpt: ""
---


### 查詢gpu使用情況
```
nvidia-smi
```

```sh
#
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.100      Driver Version: 440.100      CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce RTX 208...  Off  | 00000000:01:00.0 Off |                  N/A |
| 36%   47C    P8    19W / 250W |    932MiB / 11016MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce RTX 208...  Off  | 00000000:02:00.0 Off |                  N/A |
| 65%   64C    P2    76W / 250W |  10973MiB / 11019MiB |     33%      Default |
+-------------------------------+----------------------+----------------------+
```
<small>解釋
Gpu dirver 版本440.100  
CUDA版本建議10.2  
GPU 0號的風扇運轉36%，溫度47度c，記憶體使用932MB，GPU使用量0%
  
</small>

### 查詢cuda版本
```
nvcc --version
nvcc -V
cat /usr/local/cuda/version.txt
```
```jsx
#
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130

#
CUDA Version 10.0.130
```
<small>解釋
CUDA安裝版本為10.0.130
</small>

### ~~查詢cudnn版本~~
```
# cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

<br/><br/><br/><br/><br/>

## docker相關指令

### docker的名詞關係
docker 啟動許多虛擬電腦的工具  
image 預設好的虛擬電腦，名稱上會有其用途，表示哪些東西預先已裝好  
container 真的建立起來可跑的虛擬電腦

<br/><br/>

### 查詢docker image有哪些可以用
```
docker image ls
```
<br/><br/>

### 查詢總共有哪些建立起來的container
```
docker container ls -a
```
<br/><br/><br/><br/>


### 啟動tensorflow-gpu image的指令

```
docker run -it --rm --gpus device=0 -v ~/Documents/script:/workspace -w=/workspace tensorflow/tensorflow:1.13.1-gpu-py3 /bin/bash
```

<small>參數解釋：
`run` 從image建立container

`-it` 一定要給，才會進去container裡面

`--rm` 給了這個，一旦在container的command line輸入`exit`，container會被殺掉，<span style='color: red;'>**強烈推薦一定要加**</span>，否則記得修改container的NAME，也不要使用常見的預設port

`--gpus device=0` `--gpus device=1` `--gpus device=all` 指定了，container才看得到gpu，才可以使用gpu。`--gpus 1`不起效果

`-v 本地目錄:container內的目錄` 讓container可以看到外面，這個部分最好用Tab檢查，因為如果查無資料夾，docker root會直接在外部新增一個資料夾

`-w=/workspace` 進入container時所處的環境

`REPOSITORY:TAG` image的名稱，<span style='color: red;'>**強烈建議不用image id建立**</span>，因為會看不到container是用哪個image建立的

`/bin/bash` container內切換至command line模式
</small>

<br/><br/><br/><br/>

### 啟動tensorflow-gpu jupyter image指令

```
docker run -it --rm --gpus device=1 -p 8889:8888 -v ~/Documents/script:/workspace -w=/workspace tensorflow/tensorflow:2.0.0-gpu-py3-jupyter /bin/bash
```

<small>參數解釋：
`-p 8889:8888` 伺服器port：container port，jupyter預設port是8888，常用預設port在伺服器不要長期佔住，像6006、500以下也不要
</small>

#### Step 0. 安裝需要的package

<br/>

#### Step 1.  在當前目錄啟動jupyter notebook
```
cd /workspace
jupyter notebook --ip 0.0.0.0 --allow-root
```
<small>參數解釋：
`--ip` jupyter啟動起來類似server，在container內特別需要設定哪些電腦ip可以連上這個jupyter，0.0.0.0代表允許所有人  
`--allow-root` jupyter不建議由root啟動，但docker啟動的container預設是root身份
</small>

<br/>

#### Step 2. 打開瀏覽器
網址輸入：`localhost:8889`，預設密碼在command line啟動後印出的字串token=3d94b077dfab209ec196c8915ca3519aec4cf366e5575177

<br/>

#### Step 3. Setup a password
輸入token: `3d94b077dfab209ec196c8915ca3519aec4cf366e5575177`

new passwd `new password`

<br/><br/><br/><br/>

### 加入-\-rm，但想臨時退出，不想刪除container
```
鍵盤按 ctrl+p 再 ctrl+q
```

<br/><br/>

### 修改container名稱
```
docker container ls -a
```
```s
#
CONTAINER ID        IMAGE                                       COMMAND             CREATED             STATUS                      PORTS               NAMES
44b613d83b34        tensorflow/tensorflow:1.13.0rc1-gpu-py3     "/bin/bash"         38 hours ago        Exited (0) 2 hours ago                          suspicious_shockley
```

```
docker rename suspicious_shockley NEW_NAME
```

<br/><br/>

### 刪除container
```
docker container ls -a
```

```s
#
CONTAINER ID        IMAGE                                       COMMAND             CREATED             STATUS                      PORTS               NAMES
44b613d83b34        tensorflow/tensorflow:1.13.0rc1-gpu-py3     "/bin/bash"         38 hours ago        Exited (0) 2 hours ago                          suspicious_shockley
```
```
docker container stop suspicious_shockley
docker container rm suspicious_shockley

or

docker container stop 44b613d83b34
docker container rm 44b613d83b34
```

<br/><br/>

### 刪除image
```
docker image ls
```
```s
#
REPOSITORY              TAG                             IMAGE ID            CREATED             SIZE
local/tensorflow        1.13.0rc1-gpu-py3-cv2           e4681959890b        2 months ago        3.65GB
```

```
docker image rm e4681959890b

or

docker image rm local/tensorflow:1.13.0rc1-gpu-py3-cv2
```

<br/><br/><br/><br/>

### 進去container
#### 進入原terminal

```
docker container ls -a
```

```s
#
CONTAINER ID        IMAGE                                       COMMAND             CREATED             STATUS                      PORTS               NAMES
44b613d83b34        tensorflow/tensorflow:1.13.0rc1-gpu-py3     "/bin/bash"         38 hours ago        Exited (0) 2 hours ago                          suspicious_shockley
```

```
docker attach suspicious_shockley
再按enter
```
如果原本是啟動jupyter，看不到command line是正常的，jupyter還在啟動狀態

<br/><br/>

#### 開啟新的terminal

```
docker container ls -a
```

```s
#
CONTAINER ID        IMAGE                                       COMMAND             CREATED             STATUS                      PORTS               NAMES
44b613d83b34        tensorflow/tensorflow:1.13.0rc1-gpu-py3     "/bin/bash"         38 hours ago        Exited (0) 2 hours ago                          suspicious_shockley
```

```
docker exec -it suspicious_shockley /bin/bash
```
<small>參數解釋
`-it` 能用當前keyboard以及screen進去container操作  
`/bin/bash` 說明進去container時，是要使用command line
</small>

<br/><br/><br/><br/>

## 製作dockerfile

準備安裝清單
```
scipy
keras==2.3.0 # tensorflow 2.0+
```
<br/><br/>

### Step 1. 複製一份位於根目錄資料夾的dockerfile example，重新命名

```
ls /
cp /DockerFileCenter/dockerfile_example .
mv dockerfile_example Dockerfile
```
<small>解釋
`cp` 欲複製檔案 複製至此  
`mv` 重新命名(也有搬檔案的功用)  
製作docker image時，預設的檔案名稱就是Dockerfile</small>

```
docker image ls
```
```scala
#
REPOSITORY              TAG                             IMAGE ID            CREATED             SIZE
tensorflow/tensorflow   1.13.1-gpu-py3               e4681959890b        2 months ago        3.65GB
```
<br/><br/>

### Step 2. 打開重新命名過的Dockerfile檔案，改寫儲存
```s
FROM tensorflow/tensorflow:1.13.1-gpu-py3
MAINTAINER 你的名字

RUN pip install scipy

RUN pip install keras==2.3.0

#RUN

#ADD
#RUN
#ENV
#CMD
```
<small>參數解釋
`FROM` 使用到的 Docker Image 名稱

`MAINTAINER` 用來說明，撰寫和維護這個 Dockerfile 的人是誰，也可以給 E-mail的資訊

`RUN` RUN 指令後面放 Linux 指令，用來執行安裝和設定這個 Image 需要的東西

`ADD` 把 Local 的檔案複製到 Image 裡，如果是 tar.gz 檔複製進去 Image 時會順便自動解壓縮。Dockerfile 另外還有一個複製檔案的指令 COPY 未來還會再介紹

`ENV` 用來設定環境變數

`CMD` 在指行 docker run 的指令時會直接呼叫開啟 Tomcat Service
</small>

<br/><br/>

### Step 3. 預設在和 Dockerfile 檔案同層的資料夾底下輸入`docker image build`

```
time DOCKER_BUILDKIT=1 docker image build -t local/tensorflow:1.13.1-gpu-py3_keras . --no-cache
```
<small>`time` linux 指令，看執行程式花費的時間  
參數解釋  
`DOCKER_BUILDKIT=1` 不明  
`-t` 後面接新產出的image名稱，凡是自己做的一律改寫成local開頭  
`.` 在現在這個資料夾的意思  
`--no-cache` 避免在 Build Docker image 時被 cache 住，而造成沒有 build 到修改過的 Dockerfile
</small>

```
[+] Building 0.6s (1/7) FINISHED
 => [internal] load .dockerignore                                                                                           0.0s
 => => transferring context: 2B                                                                                             0.0s
 => [internal] load build definition from Dockerfile                                                                        0.0s
...
```

完成，照一樣的方式docker run。

<br/><br/><br/><br/>

# 常見錯誤

## OSError: [Errno 99] Cannot assign requested address

啟動jpyter notebook錯誤
```
jupyter notebook --ip 0.0.0.0 --allow-root
```
參數解釋：
`--ip` jupyter啟動起來類似server，在container內特別需要設定哪些電腦ip可以連上這個jupyter，0.0.0.0代表允許所有人  
`--allow-root` jupyter不建議由root啟動，但docker啟動的container預設是root身份

<br/><br/>

## InternalError: Blas GEMM launch failed : a.shape=(10, 6144), b.shape=(6144, 4096), m=10, n=4096, k=6144 [Op:MatMul]

沒做分配GPU Memory的動作

tensorflow 2.0+
```python
import tensorflow.compat.v1 as tf
from tensorflow.compat.v1.keras.backend import set_session
config = tf.ConfigProto()
config.gpu_options.allocator_type = 'BFC' #A "Best-fit with coalescing" algorithm, simplified from a version of dlmalloc.
config.gpu_options.per_process_gpu_memory_fraction = 0.8
config.gpu_options.allow_growth =True
```

tensorflow 1.+

```python
import tensorflow as tf
from keras.backend.tensorflow_backend import set_session
config = tf.ConfigProto()
config.gpu_options.allocator_type = 'BFC' #A "Best-fit with coalescing" algorithm, simplified from a version of dlmalloc.
config.gpu_options.per_process_gpu_memory_fraction = 0.8
config.gpu_options.allow_growth =True
```
