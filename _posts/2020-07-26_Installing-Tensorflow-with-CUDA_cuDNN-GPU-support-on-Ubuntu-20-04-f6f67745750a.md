# Installing Tensorflow with CUDA & cuDNN GPU support on Ubuntu 20.04

In this article I am installing CUDA 11 in Ubuntu 20.04. My GPU is NVIDIA GT 730. Linux kernerl v 5.4.0–42-generic. gcc (Ubuntu…

_In this article I am installing CUDA 11 in Ubuntu 20.04. My GPU is NVIDIA GT 730. Linux kernerl v_ 5.4.0–42-generic. gcc (Ubuntu 9.3.0–10ubuntu2) 9.3.0

![](https://cdn-images-1.medium.com/max/1200/1*C6BTAwfwt-Lz7ol6TM5Ajw.jpeg)

### What is CUDA ? And why we need it to perform TensorFlow’s computation with GPU?

**CUDA (Compute Unified Device Architecture)** is a parallel computing platform and programming model developed by NVIDIA on its own GPUs (graphics processing units). GPU is designed exclusively for number crunching and almost all of real world TensorFlow training data will require the full power of your GPU. CUDA enables developers to speed up compute-intensive applications by harnessing the power of GPUs for the parallelizable part of the computation.

Previously this cores only served the purpose of rendering the Graphics but since High computing requirement has arisen, Nvidia made a way to use this cores in a way GPGPU(General Purpose GPU) Computing.

**Now on the question of “Why do we actually need it?”** \- We need it because it’s an interface of communication between your application/program and GPUs. Nvidia has already implemented library called cuDNN which is only for these kind of applications. So using CUDA will give better performance.

Refer to [**this great blog**](https://datamadness.github.io/TensorFlow2-CPU-vs-GPU) for some more statistics on training models in TensorFlow in GPU vs CPU

> The 2080 RTX GPU CNN training was more than 6x faster than using the Ryzen 2700x CPU only. In other words, using the GPU reduced the required training time by 85%. This becomes far more pronounced in a real life training scenarios where you can easily spend multiple days training a single model. In this case, the GPU can allow you to train one model overnight while the CPU would be crunching the data for most of your week.

### CUDA in deep learning

Deep learning has an outsized need for computing speed. For example, to train the models for [Google Translate in 2016](https://www.nytimes.com/2016/12/14/magazine/the-great-ai-awakening.html "https://www.nytimes.com/2016/12/14/magazine/the-great-ai-awakening.html"), the Google Brain and Google Translate teams did hundreds of one-week TensorFlow runs using GPUs; they had bought 2,000 server-grade GPUs from Nvidia for the purpose. Without GPUs, those training runs would have taken months rather than a week to converge.

In addition to TensorFlow, many other DeepLearnin frameworks rely on CUDA for their GPU support, including Caffe2, CNTK, Databricks, [H2O.ai](http://H2O.ai "http://H2O.ai"), Keras, MXNet, PyTorch, Theano, and Torch. In most cases they use the cuDNN library for the deep neural network computations.

### What is cuDNN

[According to NVIDIA](https://developer.nvidia.com/cudnn#:~:text=The%20NVIDIA%20CUDA%C2%AE%20Deep,%2C%20normalization%2C%20and%20activation%20layers. "https://developer.nvidia.com/cudnn#:~:text=The%20NVIDIA%20CUDA%C2%AE%20Deep,%2C%20normalization%2C%20and%20activation%20layers.")

> “The NVIDIA CUDA® Deep Neural Network library (cuDNN) is a GPU-accelerated library of primitives for deep neural networks. cuDNN provides highly tuned implementations for standard routines such as forward and backward convolution, pooling, normalization, and activation layers.

> Deep learning researchers and framework developers worldwide rely on cuDNN for high-performance GPU acceleration. It allows them to focus on training neural networks and developing software applications rather than spending time on low-level GPU performance tuning. cuDNN accelerates widely used deep learning frameworks, including Caffe2, Chainer, Keras, MATLAB, MxNet, PyTorch, and TensorFlow.”

### CUDA Toolkit

The CUDA Toolkit includes libraries, debugging and optimization tools, a compiler, documentation, and a runtime library to deploy your applications. It has components that support deep learning, linear algebra, signal processing, and parallel algorithms.

### Important note — First check your GPU if its compatible with CUDA

You can use the `lshw` command to list the hardware installed on a Linux computer. It reports a variety of types, too—not just PCI hardware.

To tell it to report on the graphics cards it finds, we’ll use the -C (class) option and pass the “display” modifier. The -numeric option forces lshw to provide the numeric IDs of the devices, as well as their names.

Type the following:

`sudo lshw -numeric -C display`

It will give you an output something like below

```
*-display   description: VGA compatible controller   product: GP108 [GeForce GT 1030] [10DE:1D01]   vendor: NVIDIA Corporation [10DE]   physical id: 0   bus info: pci@0000:26:00.0   version: a1   width: 64 bits   clock: 33MHz   capabilities: pm msi pciexpress vga_controller bus_master cap_list rom   configuration: driver=nouveau latency=0   resources: irq:97 memory:f6000000-f6ffffff memory:e0000000-efffffff memory:f0000000-f1ffffff ioport:e000(size=128) memory:c0000-dffff
```

Take note of yours, and visit NVidia’s site of GPUs that support CUDA [**here**](https://developer.nvidia.com/cuda-gpus "https://developer.nvidia.com/cuda-gpus"). If your card is on the list, then you know that TensorFlow can support GPU operations on your machine, **but before you can install and run TensorFlow, you’ll need to install the CUDA drivers for your machine and the cuDNN updates for it.**

Alternatively you can also run

lspci | grep -i nvidia

And you will get simlar to below.

![](https://cdn-images-1.medium.com/max/800/1*DCVwww3mozUbIQH_KlwecQ.png)
undefined

#### Updating your Ubuntu 20.04 Nvidia drivers

Update your Ubuntu 20.04 Nvidia drivers to the latest (tested) versions (440 as of Jul. 2, 2020) Run Software and Updates, then select the Additional Drivers tab, and make your Nvidia selection. Various software like compilers and kernel headers should already be present for the Nvidia module to be build. When built, reboot and run nvdia-smi to endure you are running your selected Nvidia drivers.

In your browser, go to the Nvidia site, and select your CUDA version to download.

[https://developer.nvidia.com/cuda-10.2-download-archive?target\_os=Linux&target\_arch=x86\_64&target\_distro=Ubuntu&target\_version=1804&target\_type=deblocal](https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal)

### Now install CUDA Library

#### System Requirements

*   To use CUDA on your system, you will need the following installed:
*   CUDA-capable GPU
*   A supported version of Linux with a gcc compiler and toolchain
*   NVIDIA CUDA Toolkit (available at [http://developer.nvidia.com/cuda-downloads](http://developer.nvidia.com/cuda-downloads "http://developer.nvidia.com/cuda-downloads"))

### As of July-2020 — this is the official installation steps for the latest Cuda release for Ubuntu 20.04 and also the pre-installation checks.

[**cuda-installation-guide-linux/index.html**](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

[**The pdf version of the same official guide is here**](https://docs.nvidia.com/cuda/pdf/CUDA_Installation_Guide_Linux.pdf)

Now that the [NVIDIA Cuda 11 Toolkit for Ubuntu 20.04](https://developer.nvidia.com/cuda-downloads "https://developer.nvidia.com/cuda-downloads") is finally released follow the below steps.

**Very important point to remember** \- from the [**above link**](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) **please DO run the pre-installation checks and steps to make sure** your system/machine and other installed libraries meet the requirements else you need to upgrade some of the libraries or packages. (e.g. you may need to upgrade the Linux kernel )

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
```

```
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
```

```
wget http://developer.download.nvidia.com/compute/cuda/11.0.2/local_installers/cuda-repo-ubuntu2004-11-0-local_11.0.2-450.51.05-1_amd64.deb
```

```
sudo dpkg -i cuda-repo-ubuntu2004-11-0-local_11.0.2-450.51.05-1_amd64.deb
```

```
sudo apt-key add /var/cuda-repo-ubuntu2004-11-0-local/7fa2af80.pub
```

```
sudo apt-get update
```

```
sudo apt-get -y install cuda
```

Now run `$ cat /usr/local/cuda/version.txt`

It will show `CUDA Version 11.0.207`

And also to check for installed CUDA toolkit package run `$ dpkg -l | grep cuda-toolkit` the output will be something like below

```
ii  cuda-toolkit-11-0                             11.0.2-1                                  amd64        CUDA Toolkit 11.0 meta-packageii  nvidia-cuda-toolkit                           10.1.243-3                                amd64        NVIDIA CUDA development toolkit
```

And also now running `whereis cuda` will give me correctly

`cuda: /usr/lib/cuda /usr/include/cuda.h /usr/local/cuda`

### Pre-installation Actions

Some actions must be taken before the CUDA Toolkit and Driver can be installed on Linux: Verify the system has a CUDA-capable GPU. Verify the system is running a supported version of Linux. Verify the system has gcc installed. Verify the system has the correct kernel headers and development packages installed. Download the NVIDIA CUDA Toolkit. Handle conflicting installation methods.

### Install Cuda-toolkit

CUDA is a library used by many other programs like TensorFlow or OpenCV. [CUDA-toolkit](https://developer.nvidia.com/cuda-toolkit "https://developer.nvidia.com/cuda-toolkit") is an extra set software on top of CUDA which makes GPU programming with CUDA easy.

As per now, there is no deb file or run file for Ubuntu 20.04, so the only solution is to run:

```
sudo apt install nvidia-cuda-toolkit
```

It will take a while to be installed. After that, to make sure that CUDA is installed, run:

```
nvcc -V
```

`nvcc` stands for Nvidia CUDA Compiler nvcc.

You would get an output similar to the following:

```
nvcc: NVIDIA (R) Cuda compiler driver    Copyright (c) 2005-2019 NVIDIA Corporation    Built on Sun_Jul_28_19:07:16_PDT_2019    Cuda compilation tools, release 10.1, V10.1.243
```

This means that CUDA is successfully installed on your Ubuntu 20.04.

### Now lets do a comprehensive verification of all the installation that we did uptill now

#### 1\. The first method is to check the version of the Nvidia CUDA Compiler nvcc. To do so execute:

`nvcc --version`

```
nvcc: NVIDIA (R) Cuda compiler driverCopyright (c) 2005-2019 NVIDIA CorporationBuilt on Wed_Oct_23_19:24:38_PDT_2019Cuda compilation tools, release 10.2, V10.2.89
```

#### 2\. Given that you have the Nvidia driver installed on your Ubuntu 20.04 system, the following command should reveal the CUDA version:

`nvidia-smi`

I got below

![](https://cdn-images-1.medium.com/max/800/1*l1T9893SFJweOwzN5KlkAg.png)
undefined

**Note it clearly says CUDA Version: 11**

#### 3\. If you did not changed the default CUDA installation path, you can also check the CUDA version simply by viewing the content of the /usr/local/cuda/version.txt file:

```
$ cat /usr/local/cuda/version.txt
```

```
# CUDA Version 11.0.207
```

### 4\. Check for installed CUDA toolkit package:

```
$ dpkg -l | grep cuda-toolkitii  cuda-toolkit-11-0                             11.0.2-1                                  amd64        CUDA Toolkit 11.0 meta-packageii  nvidia-cuda-toolkit                           10.1.243-3                                amd64        NVIDIA CUDA development toolkit
```

### Install cuDNN — This is probably relativelyl trickier than the other steps in this whole procedures

If before installing cuDNN — you try to run a .py file containing a TensorFlow training code I was getting the following error.

```
Ubuntu 20.04 NVDIA cuda Could not load dynamic library 'libcudnn.so.7'; dlerror: libcudnn.so.7
```

### So to resolve the above you need to install cuDNN (7.6.5)

Followed this very helful [**askubuntu**](https://askubuntu.com/questions/1230645/when-is-cuda-gonna-be-released-for-ubuntu-20-04 "https://askubuntu.com/questions/1230645/when-is-cuda-gonna-be-released-for-ubuntu-20-04") **link.**

First go to [**this official link**](https://developer.nvidia.com/rdp/form/cudnn-download-survey "https://developer.nvidia.com/rdp/form/cudnn-download-survey") then choose _Download cuDNN_. You’ll be asked to login/create an account. After logging in and customary acceptance of _the Terms of the cuDNN Software License Agreement_.

A list of downloadable cuDNN will be displayed > Expand the section for Download cuDNN v7.6.5 (November 5th, 2019), for CUDA 10.1 .

**Important Note** — Do not choose the files under any other section like “cuDNN Runtime Library for Ubuntu18.04 (Deb)” or “cuDNN Developer Library for Ubuntu18.04 (Deb)”

![](https://cdn-images-1.medium.com/max/800/1*OMAdGNPWSNUGIeKJNL3WMg.png)
undefined

After the download is finished, extract the file in your locally downloaded directory

#### Then open the terminal in that downloaded-directory and run:

`sudo cp cuda/include/cudnn.h /usr/lib/cuda/include/`

To copy the `cuda/include/cudnn.h` file to `/usr/lib/cuda/include/` folder. You can of-course manually copy this file as well, i.e. without using the Terminal.

After that: copy all files under `cuda/lib64/` to `/usr/lib/cuda/lib64/` folder

```
sudo cp cuda/lib64/libcudnn* /usr/lib/cuda/lib64/
```

#### Finally: add read permission to both these target directory

```
sudo chmod a+r /usr/lib/cuda/include/cudnn.h /usr/lib/cuda/lib64/libcudnn*
```

#### Once you finish, you have to add the CUDA path to your `~/.bashrc` file. Open `~/.bashrc and add following lines`

```
echo 'export LD_LIBRARY_PATH=/usr/lib/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc    echo 'export LD_LIBRARY_PATH=/usr/lib/cuda/include:$LD_LIBRARY_PATH' >> ~/.bashrc
```

Then reload .bashrc by running:

```
source ~/.bashrc
```

### Finally to verify the correct installation

Then run `python3` and type the following lines:

```
import tensorflow as tftf.config.list_physical_devices('GPU')
```

If everything went as planned, you’ll get an output telling that Tensorflow has access to your GPU and you will see the following kind of output

```
2020-07-26 08:26:17.262278: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcuda.so.12020-07-26 08:26:17.299067: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:26:17.299364: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1561] Found device 0 with properties:pciBusID: 0000:01:00.0 name: GeForce GT 730 computeCapability: 3.5coreClock: 0.9015GHz coreCount: 2 deviceMemorySize: 3.95GiB deviceMemoryBandwidth: 11.92GiB/s2020-07-26 08:26:17.299580: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudart.so.10.12020-07-26 08:26:17.300985: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcublas.so.102020-07-26 08:26:17.301897: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcufft.so.102020-07-26 08:26:17.302118: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcurand.so.102020-07-26 08:26:17.304091: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcusolver.so.102020-07-26 08:26:17.304845: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcusparse.so.102020-07-26 08:26:17.307859: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudnn.so.72020-07-26 08:26:17.307977: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:26:17.308322: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:26:17.308579: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1703] Adding visible gpu devices: 0[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

### Also alternatively, to verify the above installation of TensorFlow and its access to GPU

Open Terminal > type `python` which will start the shell. then type

`import tensorflow as tf`

Then, ONLY to test CUDA support for your Tensorflow installation, you can run the following command in the shell:

`tf.test.is_built_with_cuda()`

Should return **True**

Finally, to confirm that the GPU is available to Tensorflow, you can test using a built-in utility function in TensorFlow as shown here:

```
tf.test.is_gpu_available(cuda_only=False, min_cuda_compute_capability=None)
```

It takes a few minutes to return a result from this; when it is finished it returns `True` and the full Terminal output will be like below.

```
2020-07-26 08:29:00.239996: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x52e4c10 initialized for platform CUDA (this does not guarantee that XLA will be used). Devices:2020-07-26 08:29:00.240016: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): GeForce GT 730, Compute Capability 3.52020-07-26 08:29:00.240194: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:29:00.240474: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1561] Found device 0 with properties:pciBusID: 0000:01:00.0 name: GeForce GT 730 computeCapability: 3.5coreClock: 0.9015GHz coreCount: 2 deviceMemorySize: 3.95GiB deviceMemoryBandwidth: 11.92GiB/s2020-07-26 08:29:00.240713: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudart.so.10.12020-07-26 08:29:00.242165: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcublas.so.102020-07-26 08:29:00.243010: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcufft.so.102020-07-26 08:29:00.243236: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcurand.so.102020-07-26 08:29:00.245184: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcusolver.so.102020-07-26 08:29:00.246093: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcusparse.so.102020-07-26 08:29:00.249772: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudnn.so.72020-07-26 08:29:00.249888: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:29:00.250286: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:29:00.250579: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1703] Adding visible gpu devices: 02020-07-26 08:29:00.250623: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudart.so.10.12020-07-26 08:29:00.251075: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1102] Device interconnect StreamExecutor with strength 1 edge matrix:2020-07-26 08:29:00.251093: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1108]      02020-07-26 08:29:00.251101: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1121] 0:   N2020-07-26 08:29:00.251213: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:29:00.251559: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:981] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero2020-07-26 08:29:00.251871: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1247] Created TensorFlow device (/device:GPU:0 with 2996 MB memory) -> physical GPU (device: 0, name: GeForce GT 730, pci bus id: 0000:01:00.0, compute capability: 3.5)True
```

### Now finally instal Tensorflow

When installing TensorFlow, we want to make sure we are installing and upgrading to the newest version available in PyPi.

Therefore, we’ll be using the following command syntax with pip:

`pip3 install --upgrade tensorflow`

It will install around 516MB of packages and then check the version like below in a .py file

```
import tensorflow as tfprint(tf.__version__)
```

Will output `2.2.0`

Alternatively, to verify the installation, you can also run the following command within the terminal directly, which will print the TensorFlow version:

`python -c 'import tensorflow as tf; print(tf.__version__)'`

#### Some other useful commands after the above full installation

**To locate all the file paths of CUDA run**

```
$ locate cuda | grep /cuda$
```

#### How to get number of GPU cards I have from a command-line

This command gets the number of GPUs directly, assuming you have nvidia-smi.

`nvidia-smi --query-gpu=name --format=csv,noheader | wc -l`

It prints the names of the GPUs, one per line, and then counts the number of lines.

#### Another issue I faced while uninstalling an old version of nvidia-cuda-toolkit before installing new version

I already had an older version of **nvidia-cuda-toolkit** packages installed from the ubuntu repo

So I did a `apt-get remove nvidia-cuda-toolkit` and then went through the instructions for installing from the cuda repo but that failed dues to broken packages (uninstalling nvidia-cuda-toolkit was not clean).

Now I’m stuck as below. Can’t remove the offending packages and also can’t fix the broken packages.

```
# apt-get remove cudaReading package lists... DoneBuilding dependency treeReading state information... DonePackage 'cuda' is not installed, so not removedYou might want to run 'apt --fix-broken install' to correct these.The following packages have unmet dependencies. cuda-libraries-10-2 : Depends: libcublas10 (>= 10.2.2.89) but 10.1.243-3 is to be installed libcublas-dev : Depends: libcublas10 (>= 10.2.2.89) but 10.1.243-3 is to be installedE: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

**Solution was to run the below command:**

`sudo apt --fix-broken install -o Dpkg::Options::="--force-overwrite"`

#### While Installing CUDA 11 in Ubuntu 20.04

After running `sudo apt-get install cuda` and I get an error about not being able to install `libnvidia-compute-450`.

So I run `sudo apt-get --fix-broken` install and I get error of the following form

```
libdvd-pkg: `apt-get check` failed, you may have broken packages. Aborting...E: Sub-process /usr/bin/dpkg returned an error code (1)error code (1)
```

Try the following to solve it by forcing its installation

`sudo apt-get -o Dpkg::Options::="--force-overwrite" install libnvidia-compute-450`

By [Rohan Paul](https://medium.com/@paulrohan) on [July 26, 2020](https://medium.com/p/f6f67745750a).

[Canonical link](https://medium.com/@paulrohan/installing-tensorflow-with-cuda-cudnn-gpu-support-on-ubuntu-20-04-f6f67745750a)

Exported from [Medium](https://medium.com) on December 12, 2020.