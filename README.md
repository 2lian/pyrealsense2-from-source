# pyrealsense2-from-source

Quick guide to build `pyrealsense2` from source for systems where binaries (`pip install pyrealsense2`) are not available.

Original documentation:
- https://dev.intelrealsense.com/docs/compiling-librealsense-for-linux-ubuntu-guide
  - The step 'Build and apply patched kernel modules' is not performed by this guide, perform it according to your needs.
- https://github.com/IntelRealSense/librealsense/tree/master/wrappers/python

## Updates, dependencies and cloning

```bash
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install libssl-dev libusb-1.0-0-dev libudev-dev pkg-config libgtk-3-dev git wget cmake build-essential python3 python3-dev  libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev python3-pybind11 at
git clone https://github.com/IntelRealSense/librealsense.git ~/librealsense
```

## Build

```bash
cd ~/librealsense 
./scripts/setup_udev_rules.sh

mkdir ~/librealsense/build 
cd ~/librealsense/build

cmake ../ -DBUILD_PYTHON_BINDINGS:bool=true -DPYTHON_EXECUTABLE=$(which python3)

make -j$(nproc)
sudo make install
```

## Preparing the python package

```bash
cp ~/librealsense/build/Release/pyrealsense2.cpython-*.so ~/librealsense/wrappers/python/pyrealsense2/
cp ~/librealsense/build/Release/librealsense2.so ~/librealsense/wrappers/python/pyrealsense2/
python3 ~/librealsense/wrappers/python/find_librs_version.py ~/librealsense/ ~/librealsense/wrappers/python/pyrealsense2/
```

Your python package is ready to be installed at: `~/librealsense/wrappers/python/`

Install it, possibly after activating a venv:
```bash
cd ~/librealsense/wrappers/python/
pip install .
```

## (Optional) Venv with pyrealsense2

```bash
sudo apt-get install python3-venv
mkdir ~/librealsense/venv
python3 -m venv ~/librealsense/venv
. ~/librealsense/venv/bin/activate
cd ~/librealsense/wrappers/python/
pip install .
```

This venv only contains `pyrealsense2`.

If installation is successful, the following command should display the root of the documentation

```bash
python3 -c "import pyrealsense2 as rs; print(rs.pyrealsense2.__doc__)"
```
```

        LibrealsenseTM Python Bindings
        ==============================
        Library for accessing Intel RealSenseTM cameras
```
