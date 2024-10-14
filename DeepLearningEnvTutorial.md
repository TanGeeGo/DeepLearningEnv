# Tutorial for the New Deep Learner in OzcanLab

## Using WSL to install Linux on your Remote Windows Desktop 

* We recommand using WSL to install a virtual Linux system (e.g., Ubuntu 22.04), and setup your deep-learning environment on it.
* [how to install Linux on Windows with WSL?](https://learn.microsoft.com/en-us/windows/wsl/install)
* Linux has the advantage over Windows on installing some C++ or CUDA related packages. [More advantages are listed here](https://www.logicmonitor.com/blog/9-reasons-linux-is-a-popular-choice-for-servers#:~:text=Linux%20has%20been%20a%20popular,quickly%20be%20identified%20and%20patched.)

## Linux operation and basic command

* After you install the virtual Linux, open your PowerShell or Windows Command Prompt. 
* In the drop-down menu above, click the name of Linux you installed. For example "Ubuntu 22.04"
* Now you are in the terminal of the new Linux system. You can also access this system with your IDE (such as VSCODE), just click the "Open a remote window" button in the left-bottom of VSCODE.

**For the command below. Please PAY ATTENTION that the `${}` represents an variable, and it shouldn't be typed in your command.**

1. Set your System Environment, install CUDA or cuDNN.
* Find the CUDA version you needed. For example, if you need to install CUDA 11.8, follow this [link](https://developer.nvidia.com/cuda-11-8-0-download-archive). After you provide the basic information of your system, it will give you the command to get the package and to install it.
```shell
# These commands are just examples, please adjust with your own system.

# this is to download the runfile from internet
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
# this is to run the file and install the package.
# NOTE: sudo command will require the permissions of root user, you need to input the password of the root user.
sudo sh cuda_11.8.0_520.61.05_linux.run
```

* Install the cuDNN according to the version of CUDA. Please follow this [link](https://developer.nvidia.com/cudnn) to install.

* After installing the CUDA and cuDNN, setup the path of these packages to your environment.
```shell
# put 'export PATH=/usr/local/cuda-11.8/bin:$PATH' this command to your .bashrc files
echo 'export PATH=/usr/local/cuda-11.8/bin:$PATH' >> ~/.bashrc
# the same as above
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
# activate the new .bashrc file
source ~/.bashrc
```

Here is the detailed introduction of .bashrc file: [What is .bashrc file in Linux](https://www.digitalocean.com/community/tutorials/bashrc-file-in-linux)

* After installing and activation, check if CUDA is successfully installed.
```shell
# confirm the environment is activate (including cuda, cudnn, and so on)
nvcc -V
# you should get the output like this:
$: nvcc: NVIDIA (R) Cuda compiler driver
$: Copyright (c) 2005-2022 NVIDIA Corporation
$: Built on Wed_Sep_21_10:33:58_PDT_2022
$: Cuda compilation tools, release 11.8, V11.8.89
$: Build cuda_11.8.r11.8/compiler.31833905_0
```

2. Set up your Python Environment
* You can follow this [instruction](https://docs.anaconda.com/anaconda/install/linux/) to download the Anaconda.

Here is an example to download the newest version (Date Oct.14.2025)
```shell
# download some pre-request packages
# `apt-get` is the install command in Linux system, just like `pip` in python
apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6

# download the anaconda into your environment
curl -O https://repo.anaconda.com/archive/Anaconda3-2024.06-1-Linux-x86_64.sh

# install
bash ~/Anaconda3-2024.06-1-Linux-x86_64.sh # check the path of Anaconda before use this command

# NOTE: the anaconda will ask you to add its path into .bashrc, please use `yes` for convenient
# You can check if the path is written to .bashrc by the command `vim .bashrc`
# in the window of `vim`, use the direction key to move up and down, use `i` to insert, use `:q` to quit vim, use `:wq` to save and quit vim.

# after anaconda path is recorded, reactivate the environment
source .bashrc

# confirm the python and conda
which python
conda list
```

3. Set up the virtual environment of Python
```shell
# create a virtual environment with the env_name and the specific python version
conda create -n ${env_name} python=${__version__} # such as `conda create -n pytorch python=3.9.6`

# enter your python environment
conda activate ${env_name}

# download the package of python
# check the CUDA version of your package first
pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 --index-url https://download.pytorch.org/whl/cu118 # cu118 means CUDA 11.8
pip install opencv-python

# ATTENTION: for pytorch, tensorflow, and JAX... We recommend you to first download these frameworks.
# Because other packages, such as numpy..., have the dependence on these frameworks. SO CHECK IT OUT FIRST! 

# run your python code
python ${pythonfile_name}
# run your python code on a specific GPU
CUDA_VISIBLE_DEVICES=0 python ${pythonfile_name}
or
CUDA_VISIBLE_DEVICES=1 python ${pythonfile_name}

# Exit your python environment
conda deactivate
```

0. Other tool for you to use
```shell
# tmux (a tool to multiple your terminal, Use tmux, turn off your PC!) 
[tmux tutorial](see http://louiszhai.github.io/2017/09/30/tmux/)

# htop (check the cpu and the pid of processes)
htop

# watch
watch -n ${refresh_time} nvidia-smi # such as watch -n 1 nvidia-smi (means every 1s print the GPU-info)

# check the storage of the disk
df -h

# check the data I/O of the disk
iostat -x ${refresh_time} # such as iostat -x 1 (means every 1s print the data I/O)

# this doc is write in markdown
[markdown tutorial](see https://github.com/guodongxiaren/README)
```

Create by Shiqi Chen
