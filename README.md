# GCP-for-ML
how to setup GPC and install related packages for ML/AI

## 2018/9/25
1. use @gmail.com login to GCP
https://console.cloud.google.com/
See console screen as
![GCP Console](https://github.com/edwardduh/GCP-for-ML/blob/master/GCP-console.jpg)

## 2018/10/16
1. Setup SSH connection to GCP/VM
use Putty
reference to http://hooterhome.pixnet.net/blog/post/136582500-gcp-vm-%E5%BB%BA%E7%AB%8B%E5%8F%8A-ssh-%E7%99%BB%E5%85%A5
PuttyGen will generate public key and private key. Need to copy/paste public key to VM SSH setting
save private key for Putty auth use

2. transfer files between Google Drive - GCP
(1). create a google CoLab .ipynb
(2). mount Google Drive on Colab/VM as /content/gd
(3). use gcloud command lines to setup connection to GCP/VM
(4). 
(5). gcloud compute scp src-files user@IP:~/dest-files

### generate ssh keys on CoLab/VM
create key under CoLab /root/.ssh/google_compute_engine, using GCP login name exxxx@gmail.com
!/usr/bin/ssh-keygen -t rsa -f /root/.ssh/google_compute_engine -C exxxx.xxx@google.com
!cat /root/.ssh/google_compute_engine.pub
  copy the public key string, and paste on GCP/VM ssh key field

!gcloud config set compute/zone asia-east1-a
!gcloud config set project powerful-gizmo-217512
!gcloud auth login
!gcloud config set account edward.duh@gmail.com

### install Anaconda, ipython, tensorflow,...
https://medium.com/@kstseng/%E5%9C%A8-google-cloud-platform-%E4%B8%8A%E4%BD%BF%E7%94%A8-gpu-%E5%92%8C%E5%AE%89%E8%A3%9D%E6%B7%B1%E5%BA%A6%E5%AD%B8%E7%BF%92%E7%9B%B8%E9%97%9C%E5%A5%97%E4%BB%B6-1b118e291015

https://hackernoon.com/launch-a-gpu-backed-google-compute-engine-instance-and-setup-tensorflow-keras-and-jupyter-902369ed5272

### install cuda
https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork

## 2018/10/17 setup ML environment
conda create -n tf python=3.6
pip install tensorflow-gpu
#pip install --upgrade pip   ## upgrade pip to 18.0
conda install keras-gpu   ## pip cannot find this package, conda will install related packages at once
## cuda toolkit and cuDNN are also included, maybe older version
pip install jupyter
pip install pillow
pip install matplotlib
pip install pytest-shutil

### Run jupyter on GCP
https://towardsdatascience.com/running-jupyter-notebook-in-google-cloud-platform-in-15-min-61e16da34d52
jupyter notebook --generate-config
### generate config file in ~/.jupyter/jupyter_notebook_config.py
vi ~/.jupyter/jupyter_notebook_config.py  # add the following statement
c = get_config()
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = <Port Number>
  
https://jeffdelaney.me/blog/running-jupyter-notebook-google-cloud-platform/
### user tmux to manage jupyter notebook when SSH terminal is inactivate due to network failure
https://stackoverflow.com/questions/47331050/how-to-run-jupyter-notebook-in-the-background-no-need-to-keep-one-terminal-f

### configure jupyter notebook fonts
on GCP VM, ~/miniconda3/envs/tf/lib/python3.6/site-packages/notebook/static/custom/custom.css

## 2018/10/18
vgg19_all-cat-tile.ipynb to train for 36 classes
(18:00) pick 10000 for train/test set, and can achieve training accuracy to 88~90%, test acc=84%
(23:00) analyze the mis-classified sample, and found 58% of failed samples are from 4 corners (position 0, 3, 12, 15)
![mis-classified tile position analysis](https://github.com/edwardduh/GCP-for-ML/blob/master/fail-tile-pos-histogram.jpg)
