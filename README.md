# Victor Lab Servers

(this document is a work in progress)

## Server List

| Name | IP | GPU | OS | Applications |
| ---- | ---- | --- |--- | ------------ |
| hal9000 | 172.28.235.138 | **RTX 3090** | Ubuntu 20 | |
| wall-e | 10.0.0.7 | RTX 3080 | Ubuntu 20, Windows 10 | |
| r2d2 | 10.0.0.4 | RTX 3080 | Ubuntu 20 | DNS |
| ava | 10.0.0.x | RTX 3080 | | |
| chappie | 10.0.0.x | RTX 3080 | | |
| t800 | 10.0.0.x | RTX 3080 | | |

## Hardware Assembly

1. Flash motherboard (if using same board) https://www.gigabyte.com/Motherboard/X570S-AORUS-ELITE-AX-rev-11/support#support-dl-bios
2. Put parts together

## Software Installation

1. Install Ubuntu (20.04.4 LTS)
   - Password should not be `password`
 
2. Install SSH server
    
    ```
    sudo apt update
    sudo apt install -y net-tools openssh-server
    sudo systemctl enable ssh
    ```
   
    Validate by opening a shell from another computer on the subnet.
   
   `ssh username@ipaddress`, "yes", `enter password`
   

also ips static etc

3. Install GPU drivers

    There doesn't seem to be a definitive online resource for every 
    command required to correctly install the necessary software for enabling 
    the GPU for computation. You should follow the instructions from Nvidia
    for the most part, however a few additional steps are listed below.
    
    1. Find the latest version for this machine.
    
        `ubuntu-drivers devices` 
        
        The above command should output something similar to ...
        
        ```shell script
        == /sys/devices/pci0000:00/0000:00:03.1/0000:0a:00.0 ==
        modalias : pci:v000010DEd00002216sv00003842sd00004897bc03sc00i00
        vendor   : NVIDIA Corporation
        driver   : nvidia-driver-510-server - distro non-free
        driver   : nvidia-driver-510 - third-party non-free
        driver   : nvidia-driver-470 - third-party non-free
        driver   : nvidia-driver-495 - third-party non-free
        driver   : nvidia-driver-515 - third-party non-free recommended
        driver   : nvidia-driver-515-server - distro non-free
        driver   : nvidia-driver-460 - third-party non-free
        driver   : nvidia-driver-470-server - distro non-free
        driver   : xserver-xorg-video-nouveau - distro free builtin
        ```
       
        Since **515** is the highest number, this will be the version we install.
    
    2. Pre install several packages that seem to not be auto installed later.
    
        ```shell script
        export version=515
        sudo apt update
        sudo apt install -y \
            libnvidia-extra-$version \
            libnvidia-compute-$version \
            nvidia-driver-$version
        ```
    
    3. Read through and follow along with Nvidia's official instructions.
    
        - https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html
        
        You can find the name of the dpkg file by navigating the distro website. 
        
        - https://developer.nvidia.com/cuda-downloads.
    
   
    ```
    sudo apt update
    sudo apt install cuda
    sudo reboot
    ```
   
    Validate by installing and running tensorflow
    
    ```shell script
    sudo apt install python3-pip
    pip install tensorflow-gpu
    echo "alias python=python3" >> ~/.bashrc && source ~/.bashrc
    python
    ```
   
    ```python
    import tensorflow as tf
    print(tf.config.list_physical_devices('GPU'))
    ```

4. Install ROS
   - ROS1 http://wiki.ros.org/noetic/Installation/Ubuntu
   - ROS2 https://docs.ros.org/en/foxy/Installation.html

