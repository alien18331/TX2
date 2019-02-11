# TX2
TX2 Ubuntu Install
  
### Iphone hotspot  
> sudo apt-add-repository ppa:pmcenery/ppa  
> sudo apt-get update  
> sudo apt-get install ipheth-utils  
  
### PIP    
> sudo apt-get update  
> sudo apt-get upgrade   
> sudo apt-get install python-pip python3-pip  
> pip -V  
> pip3 -V  
  
### OpenSSL   
Official Web: www.openssl.org/source/  
> sudo mkdir openssl  
> sudo wget "OpenSSL File"  
> sudo tar -zxvf "OpenSSL .gz File"  
  
Then move to openssl folder  
> sudo ./config --prefix=/usr/local --openssldir=/usr/local/openssl  
> sudo make  
> sudo make test  
> sudo make install  
  
### Ubuntu Python Update (3.5.2 to 3.6.6)  
> sudo wget Https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz  
> sudo tar -xvf Python-3.6.6.tgz  
  
Then move to Python-3.6.6 folder  
> ./configure --enable-optimizations  
> make  
> sudo make install  
  
Then check python3.6 loc  
> which python3.6  
return /usr/local/bin/python3.6  
  
> which python3  
return /usr/local/bin/python3  
  
> sudo ln -s /usr/local/bin/python3.6 /usr/local/bin/python3  
  
### TX2 mode  
Default mode 1  
mode / mode Name / Denver 2 /freq / ARM A57 / GPU Freq  
mode 0: max-N / 2 / 2.0 GHz / 4 / 1.30 GHz  
mode 1: Max-Q / 0 / 1.2 GHz / 4 / 0.85 GHz  
mode 2: Max-P Core-All / 2 / 1.4 GHz / 4 / 1.12 GHz  
mode 3: Max-P ARM / 0 / 2.0 GHz / 4 / 1.12 GHz  
mode 4: Max-P Denver / 2 / 2.0 GHz / 0 / 1.12 GHz  
  
Open max freq  
> cd ~  
> sudo ~/jetson_clocks.sh  
  
Check current mode  
> sudo nvpmodel -q verbose  
  
Change mode (example change to mode 0)  
> sudo nvpmodel -m 0  
  
### OpenCV (3.4.0)  
Remove all old opencv stuffs installed by JetPack (or OpenCV4Tegra)  
> sudo apt-get purge libopencv*  
  
I prefer using newer version of numpy (installed with pip), so I'd remove this python-numpy apt package as well  
> sudo apt-get purge python-numpy  
  
Remove other unused apt packages  
> sudo apt autoremove  
  
Upgrade all installed apt packages to the latest versions (optional)  
> sudo apt-get update  
> sudo apt-get dist-upgrade  
  
Update gcc apt package to the latest version (highly recommended)  
> sudo apt-get install --only-upgrade g++-5 cpp-5 gcc-5 python3-tk   
  
Install dependencies based on the Jetson Installing OpenCV Guide  
> sudo apt-get install build-essential make cmake cmake-curses-gui g++ libavformat-dev libavutil-dev libswscale-dev libv4l-dev libeigen3-dev libglew-dev libgtk2.0-dev  
  
Install dependencies for gstreamer stuffs  
> sudo apt-get install libdc1394-22-dev libxine2-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev  
  
Install additional dependencies according to the pyimageresearch article  
> sudo apt-get install libjpeg8-dev libjpeg-turbo8-dev libtiff5-dev libjasper-dev libpng12-dev libavcodec-dev  
> sudo apt-get install libxvidcore-dev libx264-dev libgtk-3-dev libatlas-base-dev gfortran  
> sudo apt-get install libopenblas-dev liblapack-dev liblapacke-dev  
  
Install Qt5 dependencies  
> sudo apt-get install qt5-default  
  
Install dependencies for python3  
> sudo apt-get install python3-dev python3-pip python3-tk  
> sudo pip3 install numpy  
> sudo pip3 install matplotlib  
  
Modify matplotlibrc (line #41) as 'backend      : TkAgg'  
> sudo vim /usr/local/lib/python3.5/dist-packages/matplotlib/mpl-data/matplotlibrc  
  
modify /usr/local/cuda/include/cuda_gl_interop.h according to   
https://devtalk.nvidia.com/default/topic/1007290/jetson-tx2/building-opencv-with-opengl-support-/post/5141945/#5141945  
> sudo vim /usr/local/cuda/include/cuda_gl_interop.h  
> cd /usr/lib/aarch64-linux-gnu/  
> sudo ln -sf tegra/libGL.so libGL.so  
  
Here’s how the relevant lines (line #62~68) of cuda_gl_interop.h look like after the modification.  
//#if defined(__arm__) || defined(__aarch64__)  
//#ifndef GL_VERSION  
//#error Please include the appropriate gl headers before including cuda_gl_interop.h   
//#endif  
//#else  
 #include <GL/gl.h>  
//#endif  
  
Next, download opencv-3.4.0 source code, cmake and compile.  
  
Download opencv-3.4.0 source code  
> mkdir -p ~/src   
> cd ~/src  
> sudo wget https://github.com/opencv/opencv/archive/3.4.0.zip -O opencv-3.4.0.zip  
> sudo unzip opencv-3.4.0.zip  
  
Build opencv (CUDA_ARCH_BIN="6.2" for TX2, or "5.3" for TX1)  
> cd ~/src/opencv-3.4.0  
> mkdir build  
> cd build  
> sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local \  
        -D WITH_CUDA=ON -D CUDA_ARCH_BIN="6.2" -D CUDA_ARCH_PTX="" \
        -D WITH_CUBLAS=ON -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON \
        -D ENABLE_NEON=ON -D WITH_LIBV4L=ON -D BUILD_TESTS=OFF \
        -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF \
        -D WITH_QT=ON -D WITH_OPENGL=ON ..  
> make -j4  
> sudo make install  
  
if Failed..  
> cd ~  
> sudo mkdir opencv  
> cd opencv  
> sudo git clone https://github.com/alien18331/buildOpenCVTX2  
> cd buildOpenCVTX2  
> sudo vim buildOpenCVTX2.sh # if want to change version  
> sudo chmod +x buildOpenCVTX2.sh     
> sudo bash ./buildOpenCV.sh -s ~/opencv # configure will generate by script  
   
To verify the installation:  
  
> ls /usr/local/lib/python3.5/dist-packages/cv2.*  
return /usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-aarch64-linux-gnu.so  
  
> python3 -c 'import cv2; print(cv2.__version__)'  
return 3.4.0  
  
### Spyder3  
> sudo apt install python3-pip python3-pyqt4 python3-pyqt5 python3-pyqt5.qtsvg python3-pyqt5.qtwebkit    
> sudo apt-get install spyder3  
> sudo apt install python3 python3-matplotlib spyder3 ipython3    
  
