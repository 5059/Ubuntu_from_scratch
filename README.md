# Ubuntu_from_scratch
(VIEW RAW FORMAT)

This is an installation guide for commonly needed libarys/programms for DL/CV (for my hardware setup). Made mainly for me to quickly setup my system and not spending days searching through forums.

## Install NVIDIA GEFORCE 970 driver 
(copied from: http://ubuntuforums.org/showthread.php?t=2263316) for backup purposes!
in terminal write

sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade && sudo apt-get install build-essential && sudo
apt-get install linux-source && sudo apt-get install linux-headers-generic

sudo gedit /etc/default/grub #change line "GRUB_CMDLINE_LINUX_DEFAULT..."-line to
GRUB_CMDLINE_LINUX_DEFAULT="nouveau.blacklist=1 quiet splash nomodeset" #(forces low-level graphics to ensure putty does not give black screen)

sudo update-grub2

sudo apt-get remove nvidia* && sudo apt-get autoremove #ensures no former installation clashes with new install

sudo reboot


After reboot get correct nvidia-driver (chose graphic-card and OS) at “http://www.nvidia.com/Download/index.aspx?lang=en-us" (or search webb with “nvidia download”). Right-click on downloaded file and change if to executable.

sudo gedit /etc/modprobe.d/blacklist.conf #add these lines at the end:
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist nvidiafb
blacklist rivatv
blacklist lbm-nouveu
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off


Open a putty-terminal with Ctrl + Alt + F1

sudo service lightdm stop #stops graphic session to enable nvidiainstallation

cd Downloads #(or wherever you downloaded your nvidia-file)

sudo ./{the downloadedfilename.run} #follow installation-instructions (normally yes to all)sudo nvidia-xconfig #(if you did not chose “yes” to this in the installation”

sudo nano /etc/default/grub # change the "GRUB_CMBLINE_LINUX_DEFAULT..."-line to below:
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset”

sudo update-grub2 #update grub!

sudo reboot


FINISHED: can be checked by nvidia-smi or lshw -c video

## Install Caffe (for that install CUDA, cudnn)
install dependencies for caffe: (see http://caffe.berkeleyvision.org/install_apt.html)
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev

### Install CUDA 
https://developer.nvidia.com/cuda-downloads from deb file and follow instructions in .pdf. 
$PATH and $LD_LIBRARY_PATH have to be written to ~/.bashrc

### Install ffmpeg 

sudo apt-add-repository ppa:mc3man/trusty-media
sudo apt-get update
sudo apt-get install ffmpeg gstreamer0.10-ffmpeg

### Install opencv 3.0.0
following  http://www.pyimagesearch.com/2015/06/22/install-opencv-3-0-and-python-2-7-on-ubuntu/
cv2.so is not copied correct. has to be done manually. is located at opencv/build and needs to get copied to dist-packages

### Install cudnn
Download .tar from Nvidia, then
tar -zxf cudnn-7.0-linux-x64-v3.0-prod.tgz
cd cuda
sudo cp lib64/* /usr/local/cuda/lib64/
sudo cp include/cudnn.h /usr/local/cuda/include/

### Caffe

git clone https://github.com/BVLC/caffe.git
cat python/requirements.txt | xargs -L 1 sudo pip install 
cp Makefile.config.example Makefile.config
edit makefile.config (uncomment use_cudnn check pythonpath, uncomment opencv_3)
make pycaffe -jX
make all -jX
make test -jX

danach tests.
./data/mnist/get_mnist.sh
./examples/mnist/create_mnist.sh
./examples/mnist/train_lenet.sh

for working import in python:
export PYTHONPATH=<caffe-home>/python:$PYTHONPATH <-- to /.bashrc

if: error libopencv_core.so.3.0 not found. create link/copy files from opencv/build/lib to /usr/local/lib


## Install spyder
via pip install spyder




for jekyll: installed ruby: ruby 2.3.0 and 2.3.0-dev installed
