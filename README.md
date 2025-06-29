# Installation Guide for Nektar++

## Building OCCT Library

OpenCascade library is required for mesh generation and optimization in Nektar++. The steps below are used to compile the OCCT library from source.  The installation path for the library is set to **$HOME/occt-install**. 

> [!NOTE]
> You may skip this step if you have disabled the `NEKTAR_USE_MESHGEN` option.

- Install the dependencies of OCCT library
```sh
sudo apt update && sudo apt install -y \
    build-essential cmake git \
    freeglut3-dev libx11-dev libxext-dev \
    libxt-dev libgl1-mesa-dev libglu1-mesa-dev \
    libfreetype6-dev libtbb-dev \
    libxmu-dev libxi-dev \
    libpng-dev libglfw3-dev \
    libeigen3-dev libfontconfig1-dev
```

- Download the source code of OCCT
```sh
wget https://github.com/Open-Cascade-SAS/OCCT/archive/refs/tags/V7_9_1.tar.gz
```

- Extract and enter the OCCT folder
```sh
tar -xvzf V7_9_1.tar.gz && cd OCCT-7_9_1
```

- Create a build directory and enter it.
```sh
mkdir build && cd build
```

- Configure the build using cmake.
```sh
cmake .. \
  -DCMAKE_INSTALL_PREFIX=$HOME/occt-install -DBUILD_MODULE_Draw=OFF \
  -DBUILD_MODULE_ApplicationFramework=ON -DBUILD_MODULE_DataExchange=ON \
  -DBUILD_MODULE_ModelingData=ON -DBUILD_MODULE_ModelingAlgorithms=ON \
  -DBUILD_MODULE_Visualization=ON -DCMAKE_BUILD_TYPE=Release
```

- Install the library
```sh
make -j4 install
```

- Export the path to installation location, so that Nektar++ installer can identify it.
```sh
export OpenCASCADE_DIR=$HOME/occt-install
export LD_LIBRARY_PATH=$HOME/occt-install/lib:$LD_LIBRARY_PATH
```

## Installing other dependencies

```sh
sudo apt install libboost-all-dev liblapack-dev libblas-dev libblas-dev libscotch-dev libtinyxml-dev libptscotch-dev libmetis-dev libarpack2-dev
```

## Download the source code

```sh
wget https://www.nektar.info/src/nektar%2B%2B-5.8.0.tar.gz && tar -xvzf nektar++-5.8.0.tar.gz
```

## Create the build directory

```sh
cd nektar-v5.8.0 && mkdir -p build && cd build
```

## Configure the installation

- Configure the installation by running `ccmake ../`
	- Press c to start the configuration.
	- Select the components of Nektar++ (prefixed with `NEKTAR_BUILD_`) you would like to build. Disabling solvers which you do not require will speed up the build process.
	- Select the optional libraries you would like to use (prefixed with `NEKTAR_USE_`) for additional functionality. Check the list of configuration options can be accessed [here](https://doc.nektar.info/userguide/latest/user-guidese3.html#x7-180001.3.5).
	- Press c to continue the configuration.
	- Press g to generate the make files.

![CMake GUI](cmake.png)

## Compile the code

```sh
make -j4 install
```

## Add to path and test
After completion of installation, add the bin and lib64 folders present in the default installation folder `nektar-v5.8.0/build/dist` to PATH.

```sh
export PATH=$PATH:HOME/nektar-v5.8.0/build/dist/bin
export LD_LIBRARY_PATH=$HOME/nektar-v5.8.0/build/dist/lib64:$HOME/occt-install/lib:$LD_LIBRARY_PATH
ctest
```