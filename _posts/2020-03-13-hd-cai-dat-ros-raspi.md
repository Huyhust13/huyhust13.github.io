---
layout: post
title: Hướng dẫn cài đặt ROS trên Raspberry Pi
comment: true
categories: [blog, ROS]
tags: [ros, raspberry]
---
[writing...]

Trong bài này, mình sẽ hướng dẫn các bạn cài đặt ROS Melodic trên Raspberry Pi 3.
Sau mấy ngày mày mò cài đi cài lại các hệ điều hành trên Raspberry Pi để cài ROS, cuối cùng mình lựa chọn Ubuntu MATE để dừng chân. Tuy nó không tối ưu khi chạy trên Raspberry Py bằng 
# Cài đặt Ubuntu Mate

    Thời điểm hiện tại là Ubuntu MATE 18.04 [tại đây](https://ubuntu-mate.org/download/)
    Phiên bản ROS tương ứng là ROS melodic (* Nói thêm về các phiên bản ROS tương ứng với phiên bản hệ điều hành, sự tương thích giữa các phiên bản)
    

# Cài đặt ROS
## Hướng dẫn cài đặt
...
## Một số lưu ý
- Nếu trong qúa trình `catkin_make`, xảy ra lỗi `c++: internal compiler error: Killed (program cc1plus)` thì thực hiện lại lệnh `catkin_make -j2`
# Cấu hình Pi thông dụng
## Xóa/gỡ LibreOffice:
    ```
    sudo apt-get remove --purge libreoffice*
    sudo apt-get clean
    sudo apt-get autoremove
    ```
## Cài đặt VNC (Remote từ xa, giống Teamview)
- Tải realVNC-server cho Raspberry Pi [tại đây](https://www.realvnc.com/en/connect/download/vnc/raspberrypi/)
- Cài đặt:
```
sudo dpkg -i path/to/file
```
- Enable correct service:
    - To access local session, use Service mode: `sudo systemctl enable vncserver-x11-serviced.service`
    - To access a virtual session, use Virtual mode: `sudo systemctl enable vncserver-virtuald.service`
- reboot.
        
## Cài đặt ssh-server
- `sudo raspi-config` để mở bảng cấu hình raspi
- Chọn vào `Interfacing Options` -> Kích hoạt ssh

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

## Cấu hình boot:
Mở file `boot/config.txt`
Đặt:
```
hdmi_force_hotplug = 1
hdmi_mode = 16 (chọn theo cấu hình màn hình)
```
Mình thấy khi chọn thông số  `hdmi_mode` không phù hợp thì pi không khởi động được.

# Cài đặt dashgo



# Tham khảo:
1. https://roboticsbackend.com/install-ros-on-raspberry-pi-3/
2. https://www.intorobotics.com/installing-ros-melodic-on-raspberry-pi-3b-running-ubuntu-mate-18-04-2-bionic/
3. 