---
layout: post
title: Hướng dẫn cài đặt ROS trên Raspberry Pi
comment: true
categories: [blog, ROS]
tags: [ros, raspberry]
---

<!-- TOC -->

- [Lựa chọn phiên bản ROS và phiên bản hệ điều hành Linux](#lựa-chọn-phiên-bản-ros-và-phiên-bản-hệ-điều-hành-linux)
- [Cài đặt Ubuntu Mate](#cài-đặt-ubuntu-mate)
    - [Download](#download)
    - [Cài đặt Ubuntu MATE](#cài-đặt-ubuntu-mate)
        - [Trên Ubuntu](#trên-ubuntu)
- [Cấu hình Pi thông dụng](#cấu-hình-pi-thông-dụng)
    - [Xóa/gỡ LibreOffice:](#xóagỡ-libreoffice)
    - [Cài đặt VNC (Remote từ xa, giống Teamview)](#cài-đặt-vnc-remote-từ-xa-giống-teamview)
    - [Cài đặt ssh-server](#cài-đặt-ssh-server)
- [Cài đặt ROS](#cài-đặt-ros)
    - [Hướng dẫn cài đặt](#hướng-dẫn-cài-đặt)
        - [Cấu hình repositorys:](#cấu-hình-repositorys)
    - [Một số lưu ý](#một-số-lưu-ý)
    - [Cấu hình boot:](#cấu-hình-boot)
- [Tham khảo](#tham-khảo)

<!-- /TOC -->

## Lựa chọn phiên bản ROS và phiên bản hệ điều hành Linux
Trong bài này, mình sẽ hướng dẫn các bạn cài đặt ROS trên Raspberry Pi 3.
Sau mấy ngày mày mò cài đi cài lại các hệ điều hành trên Raspberry Pi để cài ROS, cuối cùng mình lựa chọn Ubuntu MATE 16.04, ROS Kinetic Kame. 

Sự tương thích giữa phiên bản ROS và Linux như sau:

![ros-linux](/img/ROS-vs-linux.png)


## Cài đặt Ubuntu Mate
### Download 
Link download Ubuntu Mate 16.04 [link download](https://ubuntu-mate.org/raspberry-pi/ubuntu-mate-16.04.2-desktop-armhf-raspberry-pi.img.xz)
Phiên bản ROS tương ứng là Kinetic 

Trong bài này, mình sẽ hướng dẫn các bạn cài ROS Kinetic trên nền tảng hệ điều hành Ubuntu Mate 16.04

Thời điểm hiện tại là Ubuntu MATE 18.04 [tại đây](https://ubuntu-mate.org/download/)
Phiên bản ROS tương ứng là ROS melodic (* Nói thêm về các phiên bản ROS tương ứng với phiên bản hệ điều hành, sự tương thích giữa các phiên bản)

### Cài đặt Ubuntu MATE
#### Trên Ubuntu
- [Download Etcher](https://www.balena.io/etcher/)
- Giải nén file vừa tải về
- Kích đúp vào file vừa giải nén
![](/img/balenaEtcher.png)
- Chọn file cài đặt Ubuntu Mate vào `select Image`, chọn thẻ nhớ cài đặt vào `Select target` sau đó nhấn vào `Flash!` và chờ đến khi flash xong.

## Cấu hình Pi thông dụng
### Xóa/gỡ LibreOffice:
```
sudo apt-get remove --purge libreoffice*
sudo apt-get clean
sudo apt-get autoremove
```

### Cài đặt VNC (Remote từ xa, giống Teamview)
- Tải realVNC-server cho Raspberry Pi [tại đây](https://www.realvnc.com/en/connect/download/vnc/raspberrypi/)
- Cài đặt:
```
sudo dpkg -i path/to/file
```
- Khởi đông vnc:
    - To access local session, use Service mode: `sudo systemctl enable vncserver-x11-serviced.service`
    - To access a virtual session, use Virtual mode: `sudo systemctl enable vncserver-virtuald.service`
- reboot.
        
### Cài đặt ssh-server
- `sudo raspi-config` để mở bảng cấu hình raspi
- Chọn vào `Interfacing Options` -> Kích hoạt ssh

## Cài đặt ROS
### Hướng dẫn cài đặt
#### Cấu hình repositorys:
**Bước 1:** Vào System -> Administration -> Software & Updates

**Bước 2:** Tích vào checkbox như hình dưới:

![software](/img/Software-and-Updates_opt.png)

**Bước 3:** Thiết lập sources.list

Mở terminal:

```
sudo sh -c ‘echo “deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main” > /etc/apt/sources.list.d/ros-latest.list’
```

**Bước 4:** Thiết lập keys:

```
wget http://packages.ros.org/ros.key -O – | sudo apt-key add –
```

**Bước 5:** Cập nhật ubuntu:

```
sudo apt-get update
```

**Bước 6:** Cài đặt Ros Kinetic bản đầy đủ:

```
sudo apt-get install ros-kinetic-desktop-full
```

(Chờ một lúc, nhấn `y` để tiếp tục cài, chờ tiếp ^-^)

**Bước 7:** Khởi tạo rosdep

```
sudo rosdep init
rosdep update
```

**Bước 8:** Cài đặt biến môi trường cho ROS

```
echo “source /opt/ros/kinetic/setup.bash” >> ~/.bashrc
source ~/.bashrc
```

**Bước 9:** Khởi tạo không gian làm việc catkin (`catkin_ws`)

```
mkdir -p ~/catkin_ws/src
cd catkin_ws/src
catkin_init_workspace
cd ~/catkin_ws/
catkin_make
```

**Bước 10:** Thêm `catkin_ws` vào biến môi trường ROS

```
source ~/catkin_ws/devel/setup.bash
echo “source ~/catkin_ws/devel/setup.bash” >> ~/.bashrc
```

**Bước 11:** Kiểm tra các biến môi trường ROS

```
export | grep ROS
```

### Một số lưu ý
- Nếu trong qúa trình `catkin_make`, xảy ra lỗi `c++: internal compiler error: Killed (program cc1plus)` thì thực hiện lại lệnh `catkin_make -j2`

**Lưu ý một số lỗi gặp phải:**
Truy cập từ xa qua ssh:
Tại remote PC: `ssh <username>@<ip>`

- Lỗi 01: `Connection reset by 192.168.1.200 port 22`
    
    Trên Pi: 
    ```
    sudo rm /etc/ssh/ssh_host_*
    sudo dpkg-reconfigure openssh-server
    ```
    Sau đó kết nối lại bình thường

### Cấu hình boot:
Mở file `boot/config.txt`
Đặt:
```
hdmi_force_hotplug = 1
hdmi_mode = 16 (chọn theo cấu hình màn hình)
```
Mình thấy khi chọn thông số  `hdmi_mode` không phù hợp thì pi không khởi động được.

## Tham khảo
1. https://roboticsbackend.com/install-ros-on-raspberry-pi-3/
2. https://www.intorobotics.com/installing-ros-melodic-on-raspberry-pi-3b-running-ubuntu-mate-18-04-2-bionic/