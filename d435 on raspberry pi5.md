# Guide for using the Intel RealSense D435 on the Raspberry Pi 5

# Installation Pre-Requirements 
  - Update, upgrading, and installing packages
  ```
  sudo apt-get update && sudo apt-get upgrade
  sudo reboot
  sudo apt-get install automake libtool vim cmake libusb-1.0-0-dev libx11-dev xorg-dev libglu1-mesa-dev
  ```
  - Expand file system to allow full use of sd
  ```
  sudo raspi-config
  ```
  Go to `Advanced Options` using arrow keys, press enter, then press enter again to go to `A1 Expand Filesystem`. Say `YES` to rebooting now
- Increase swap to 2GB  
  run `sudo vi /etc/dphys-swapfile`, then go to the line that says `CONF_SWAPSIZE` and set it equal to `2048` with this line `CONF_SWAPSIZE=2048`
  - Apply the changes you just made
    ```
    sudo /etc/init.d/dphys-swapfile restart swapon -s
    ```
- Create a new `udev` rule
  ```
  cd ~
  git clone https://github.com/IntelRealSense/librealsense.git
  cd librealsense
  sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/ \
  ```
- Apply the change (needs to be run by super user):
  ```
  sudo su
  udevadm control --reload-rules && udevadm trigger
  exit
  ```
- Modify the path by adding to the `.bashrc` file
  ```
  echo 'export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH' >> ~/.bashrc
  ```
- Apply the change
    ```
    source ~/.bashrc
    ```
- Install snapd
  ```
  sudo apt install snapd
  sudo reboot
  ```
  ```
  sudo snap install snapd
  ```
# Installation
- Install protobuf
  ```
  sudo snap install protobuf --classic
  ```
- Install Cmake
  ```
  sudo snap install cmake --classic
  ```
- Install `libtbb-dev` parallelism library
  ```
  cd ~
  wget https://github.com/PINTO0309/TBBonARMv7/raw/master/libtbb-dev_2018U2_armhf.deb
  sudo dpkg -i ~/libtbb-dev_2018U2_armhf.deb
  sudo ldconfig
  rm libtbb-dev_2018U2_armhf.deb
  ```
- Install the RealSense SDK
  ```
  cd ~/librealsense
  mkdir  build  && cd build
  cmake .. -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=Release -DFORCE_LIBUVC=true
  make -j1
  sudo make install
  ```
- Modify the path
  ```
  echo 'export PYTHONPATH=$PYTHONPATH:/usr/local/lib' >> ~/.bashrc
  ```
- Apply the change
  ```
  source ~/.bashrc
  ```
- Install OpenGL
  ```
  sudo pip3 install pyopengl
  ```
- Enable VNC
  ```
  sudo raspi-config
  ```
  Go to `Interface Options`, go to `VNC`, then enable it
- Test it and see!!!
  - Go to any VNC viewer, I use tiger VNC, connect to your pi
  - Run `realsense-viewer` in a terminal window from your VNC
  - Test and see!!
  ![image](https://github.com/user-attachments/assets/a760787f-63f3-4130-ba71-45e8acdd2071)  
