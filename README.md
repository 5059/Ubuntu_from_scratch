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

first some general dependencies:
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


install dependencies for caffe: (see http://caffe.berkeleyvision.org/install_apt.html)



compiling caffe with changed makefile.config: USE_CUDNN :=1 OPENCV_VERSION:=3. PYTHON_INCLUDE

for jekyll: installed ruby: ruby 2.3.0 and 2.3.0-dev installed
