There seems to be some issues with Anaconda2 and OpenCV3, especially with the ffmpeg bindings. As they are important for me and I couldn't get them running with anaconda's python, everything is made for the preinstalled python.

# System update

```
sudo apt-get update
sudo apt-get upgrade
```

# Install NVIDIA Driver (GTX 970)

to be added, when setting up my private workstation.

# Install git

```
sudo apt-get install git
```

If later on error's are encountered with runit and git-daemon-run do:

```
sudo apt-get purge runit
sudo apt-get purge git-all
sudo apt-get purge git
sudo apt-get autoremove
sudo apt-get update
sudo apt-get install git
```

# Installing ffmpeg

In 16.04 it's simple again, installing via

```
sudo apt-get install ffmpeg
```

# Installing OpenCV3

Actual version is 3.2.0, installation follows the guide of [pyimagesearch](http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/) but is copied for backup reasons and modified a bit (I install opencv globally, as I need it in nearly all of my projects).

Prerequisites:

```
sudo apt-get install build-essential cmake pkg-config
sudo apt-get install libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libgtk-3-dev
sudo apt-get install libatlas-base-dev gfortran
sudo apt-get install python2.7-dev
sudo apt-get install python-pip
sudo pip install --upgrade pip
sudo pip install numpy
```

Download OpenCV 3.2.0 and Contrib-Package:
```
wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.2.0.zip
unzip opencv.zip
wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.2.0.zip
unzip opencv_contrib.zip
```

Build/Install OpenCV
```
cd /opencv-3.2.0/
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D INSTALL_PYTHON_EXAMPLES=ON \
      -D INSTALL_C_EXAMPLES=OFF \
      -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.2.0/modules \
      -D BUILD_EXAMPLES=ON ..
make -j8
sudo make install
```

Then test correct installation and Video support by
```
cd
python

#inside python
import cv2
cv2.__version__ #should print '3.2.0'

vid = cv2.VideoCapture('test.avi') #load some test video
ret, frame = vid.read()
ret #should output True inside the console
cv2.imwrite('test.png', frame) #should save the frame as image
```

# Install Caffe (Berkely's DL library)
Install dependencies see [here](http://caffe.berkeleyvision.org/install_apt.htm)
```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```
Clone Caffe's git repo and install python requirements
```
git clone https://github.com/BVLC/caffe.git
cd caffe
cat python/requirements.txt | xargs -L 1 sudo -H pip install
cp Makefile.config.example Makefile.config
```
Edit the Makefile.config and change following points:
- uncomment CPU_ONLY := 1 (on machines w/o support graphic card)
- uncomment OPENCV_VERSION := 3
- Check Python Paths
- Change INCLUDE_DIRS and LIBRARY_DIRS under #Whatever else your find.... to
  - INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
  - LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/
- Change PYTHON_INCLUDE to correctly point to numpy/arrayobject.h (could be in /usr/local/lib/.. insted of /usr/lib/...)

Install/Build Caffe
```
make pycaffe -j8
make all -j7
make test -j8
```
Run tests to see if everything is correctly installed
```
./data/mnist/get_mnist.sh
./examples/mnist/create_mnist.sh
./exmaples/mnist/train_lenet.sh
```
If CPU_ONLY installation change /examples/mnist/lenet_solver.prototxt to: solver_mode = CPU

To work as a python importable package, we have to adapt the PYTHON_PATH variable
-export PYTHONPATH=<caffe-home>/python:$PYTHONPATH <-- to /.bashrc

# Spyder IDE

Install spyder 2.3.9 with all optional dependencies. I had troubles using spyder 3, so this is how to setup spyders latest 2.3.9 version.
```
sudo apt-get install python-qt4 #required
sudo -H pip install ipython jedi==0.8.1 matplotlib pandas pep8 psutil pyflakes pygments
sudo -H pip install pylint qtconsole rope sphinx sympy zmq
sudo pip install spyder==2.3.9 #having troubles with spyder3 (debug object inspection)
```

If spyder, started from Launcher isn't able to import caffe add:
- <path to caffe>/python
to spyders path manager and restart spyder

# Tensorflow, Theano and Keras
```
sudo -H pip install tensorflow keras #keras will install theano as dependencie
```
