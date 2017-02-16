## Installing CAFFE with python wrapper, UBUNTU - 16.04, CPU mode only
This is blog post to install CAFFE on ubuntu 16.04 with CPU only

## Check for dependencies : 

required gcc -- version 5+

required g++ -- version 5+ 

## Installing dependencies : 

sudo apt-get update 

sudo apt-get upgrade

sudo apt-get install -y build-essential cmake git pkg-config

sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler

sudo apt-get install -y libatlas-base-dev 

sudo apt-get install -y --no-install-recommends libboost-all-dev

sudo apt-get install -y libgflags-dev libgoogle-glog-dev liblmdb-dev

sudo apt-get install -y python-pip

### (Python 2.7 development files)
sudo apt-get install -y python-dev

sudo apt-get install -y python-numpy python-scipy

### (or, Python 3.5 development files)
sudo apt-get install -y python3-dev

sudo apt-get install -y python3-numpy python3-scipy

### (OpenCV 2.4)
sudo apt-get install -y libopencv-dev

### (OpenCV 3.0.1)
Look here : https://github.com/BVLC/caffe/wiki/OpenCV-3.1-Installation-Guide-on-Ubuntu-16.04

## Creating Caffe directory and getting caffe files 
mkdir deep-learning

cd ~/deep-learning

git clone http://github.com/BVLC/caffe.git 

cd caffe 

## Few changes into the Make-files

Copy the Makefile.config.example for future reference
cp Makefile.config.example Makefile.config

Do the following changes to the Makefile.config file: 

1. Uncomment the CPU_ONLY line, if installing using CPU only 
     CPU_ONLY := 1
     
2. So make sure you note the following changes to the include_dirs and library_dirs
     WITH_PYTHON_LAYER := 1
     
3. Replace the lines having INCLUDE_DIRS and LIBRARY_DIRS by the lines below: 

  `INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial`

  `LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial`

4. Make symbolic links: 

  - ```find . -type f -exec sed -i -e 's^"hdf5.h"^"hdf5/serial/hdf5.h"^g' -e 's^"hdf5_hl.h"^"hdf5/serial/hdf5_hl.h"^g' '{}' \;```

  - ```cd /usr/lib/x86_64-linux-gnu```

  - ```sudo ln -s libhdf5_serial.so.10.1.0 libhdf5.so```

  - ```sudo ln -s libhdf5_serial_hl.so.10.0.2 libhdf5_hl.so  ```

5. Install Python requirements: 

    ```cd python```
    
    ```for req in $(cat requirements.txt); do sudo pip install $req; done ```

    In case of any problems, try:
    
    ```for req in $(cat requirements.txt); do sudo -H pip install $req --upgrade; done```
    
    ```cd ..```

6. Do the following changes to the Makefile file:

    Replace the line 181 (probably) starting with LIBRARIES by the line below :

    ```LIBRARIES += glog gflags protobuf leveldb snappy \
      lmdb boost_system boost_filesystem hdf5_hl hdf5 m \
      opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs opencv_videoio```

## Build CAFFE

```make all```

```make test```

```make runtest```

## Build Pycaffe
```make pycaffe```

## Check the installation
    Import CAFFE in python as shown below:
    
    cd python
    
    python
    
    >>> import caffe
    >>>



