
# darknet install

git clone https://github.com/AlexeyAB/darknet
cd darknet
mkdir build_release
cd build_release
cmake ..

	To update CMake on Ubuntu, it's better to follow guide here: https://apt.kitware.com/ or https://cmake.org/download/


	https://askubuntu.com/questions/1203635/installing-latest-cmake-on-ubuntu-18-04-3-lts-run-via-wsl-openssl-error

	sudo apt install cmake

	nvidia@DESKTOP-KPVQIH1:~/darknet/build_release$ cmake ..
	CMake Error at CMakeLists.txt:1 (cmake_minimum_required):
	  CMake 3.18 or higher is required.  You are running version 3.10.2

	sudo apt-get update
	sudo apt-get install apt-transport-https ca-certificates gnupg software-properties-common wget
	wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | sudo apt-key add -

	sudo apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'
	sudo apt-get update
	sudo apt-get install cmake


cmake ..
>>>>>>>>>>>>>>>>> 
CMake Error at CMakeLists.txt:197 (find_package):
  By not providing "FindOpenCV.cmake" in CMAKE_MODULE_PATH this project has
  asked CMake to find a package configuration file provided by "OpenCV", but
  CMake did not find one.

sudo apt install libopencv-dev

cmake ..
>>>>>>>>>>>>>>>>> 
--   ->  darknet is fine for now, but uselib_track has been disabled!
--   ->  Please rebuild OpenCV from sources with CUDA support to enable it
CMake Error at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:230 (message):
  Could NOT find CUDNN (missing: CUDNN_INCLUDE_DIR CUDNN_LIBRARY)
Call Stack (most recent call first):
  /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:594 (_FPHSA_FAILURE_MESSAGE)
  cmake/Modules/FindCUDNN.cmake:78 (find_package_handle_standard_args)
  CMakeLists.txt:289 (find_package)

https://wenyuangg.github.io/posts/opencv/opencv-installation.html
https://cloud.tencent.com/developer/article/1657529
二、 从源码安装 OpenCV

cd ~
https://github.com/opencv/opencv/releases/tag/4.5.4
tar zxvf opencv-4.5.4.tar.gz

cd ~
mv opencv-4.5.4 opencv
cd opencv
mkdir build && cd build

cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=ON \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_GENERATE_PKGCONFIG=ON \
	-D BUILD_EXAMPLES=ON ..
	
	
	#-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \



make -j8

sudo make install

cmake ..
>>>>>>>>>>>>>>>>> 
--   ->  darknet is fine for now, but uselib_track has been disabled!
--   ->  Please rebuild OpenCV from sources with CUDA support to enable it
CMake Error at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:230 (message):
  Could NOT find CUDNN (missing: CUDNN_INCLUDE_DIR CUDNN_LIBRARY)
Call Stack (most recent call first):
  /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:594 (_FPHSA_FAILURE_MESSAGE)
  cmake/Modules/FindCUDNN.cmake:78 (find_package_handle_standard_args)
  CMakeLists.txt:289 (find_package)



https://developer.nvidia.com/rdp/cudnn-archive
>>>>>>>>>>>>>>>>> Method1: Wsl2 Install Cudnn ----------- Fail
cp /mnt/d/Install_Packages/NV_Driver/libcudnn8_8.2.4.15-1+cuda11.4_amd64.deb ./
sudo dpkg -i libcudnn8_8.2.2.26-1+cuda11.4_amd64.deb

>>>>>>>>>>>>>>>>> Method2: Wsl2 Install Cudnn 
https://zhuanlan.zhihu.com/p/350399229
cp /mnt/d/Install_Packages/NV_Driver/cudnn-11.4-linux-x64-v8.2.4.15.tgz ./
tar -zxvf cudnn-11.4-linux-x64-v8.2.4.15.tgz

https://github.com/pjreddie/darknet/issues/2356
marfis89 commented on 8 Jan
	New cmd :
	sudo cp cuda/include/cudnn*.h /usr/local/cuda/include  
	sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64  

sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-11.4/lib64/
sudo cp  cuda/include/cudnn*.h /usr/local/cuda-11.4/include/

sudo chmod a+r /usr/local/cuda-11.4/include/cudnn*.h
sudo chmod a+r /usr/local/cuda-11.4/lib64/libcudnn*

sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-11/lib64/
sudo cp  cuda/include/cudnn*.h /usr/local/cuda-11/include/

sudo chmod a+r /usr/local/cuda-11/include/cudnn*.h
sudo chmod a+r /usr/local/cuda-11/lib64/libcudnn*

sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo cp  cuda/include/cudnn*.h /usr/local/cuda/include/

sudo chmod a+r /usr/local/cuda/include/cudnn*.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*


cmake ..
cmake --build . --target install --parallel 8

Segmentation fault
CMakeFiles/dark.dir/build.make:1058: recipe for target 'CMakeFiles/dark.dir/src/activation_kernels.cu.o' failed

>>>>>>>>>>>>>>>>>  關關難過-----關關過-----

https://stackoverflow.com/questions/58445944/segmentation-fault-when-compiling-darknet-for-gpu
https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions

export PATH=/usr/local/cuda-11.4/bin${PATH:+:${PATH}} >> ~/.bashrc

再次確認 Makefile
# GeForce RTX 3070, 3080, 3090
ARCH= -gencode arch=compute_86,code=[sm_86,compute_86]

cd ~/darknet/build_release
rm -r *

cmake ..
cmake --build . --target install --parallel 8

make[1]: Entering directory '/home/nvidia/darknet/build_release'
make[1]: Nothing to be done for 'preinstall'.
make[1]: Leaving directory '/home/nvidia/darknet/build_release'
Install the project...
/usr/bin/cmake -P cmake_install.cmake
-- Install configuration: "Release"
-- Installing: /home/nvidia/darknet/libdarknet.so
-- Set runtime path of "/home/nvidia/darknet/libdarknet.so" to ""
-- Installing: /home/nvidia/darknet/include/darknet/darknet.h
-- Installing: /home/nvidia/darknet/include/darknet/yolo_v2_class.hpp
-- Installing: /home/nvidia/darknet/uselib
-- Set runtime path of "/home/nvidia/darknet/uselib" to ""
-- Installing: /home/nvidia/darknet/darknet
-- Set runtime path of "/home/nvidia/darknet/darknet" to ""
-- Installing: /home/nvidia/darknet/share/darknet/DarknetTargets.cmake
-- Installing: /home/nvidia/darknet/share/darknet/DarknetTargets-release.cmake
-- Installing: /home/nvidia/darknet/share/darknet/DarknetConfig.cmake
-- Installing: /home/nvidia/darknet/share/darknet/DarknetConfigVersion.cmake

>>>>>>>>>>>>>>>>>  成功 ^^

