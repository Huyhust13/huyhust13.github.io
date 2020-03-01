---
layout: post
title: Sổ tay Ubuntu
comment: true
---
# Lệnh thường dùng linux

- Hiện toàn bộ ổ cứng: `df -h`
- Truyền file qua mạng LAN sftp(security file transfer  protocol):
    - Connect:  `$ sftp user@192.xxx.xxx.xxx`
- Truyền file qua pscp:
`pscp path\remote\file userid@ip:/tmp/foo/info.txt`
- Trước tiên phải cài: `sudo apt-get install openssh-server`

### File nén:
- Giải nén file đã được chia nhỏ:

    - `cat splitted_zip_file.zip* > zip_file.zip` để  nối các file .zip.xxx thành 1 file lớn.
    (Lệnh này có thể dẫn tới việc bộ nhớ đệm của gedit bị tăng lên, sau khi thực hiện xong, mở system monitor để kiểm tra và kill processing)
    - `unzip zip_file.zip` để giải nén

### Phím tắt
- Chụp màn hình 1 vùng:  <Shift + Print>
- Chụp một vùng màn hình và copy luôn: <Shift + Ctrl + Print>

### Ưu tiên mạng khi dùng nhiều card mạng 
- `route -n` để hiện các card mạng
- `ifconfig <ten_card_mang> <ip>`: đặt ip cho card mạng đó.
- `ifmetric <ten_card_mang> <so_metric>`: thiết lập metric, số metric càng thấp thì ưu tiên càng cao.

### Uninstall app:
`sudo apt-get --purge remove program`

### Install nvidia driver home pc (ubuntu 16.04 + gtx1650)
Step-1: Obviously starting with an update and upgrade

```
sudo apt-get update
sudo apt-get upgrade
```

Step-2: Then remove all Nvidia packages (skip this if your OS is fresh installed) :
```
sudo apt-get remove nvidia*
sudo apt autoremove
```
Step-3: Install these packages for building the kernel:
```
sudo apt-get install dkms build-essential linux-headers-$(uname -r)
```
Step-4: Now block and disable Nouveau kernel driver:
```
echo "# Disable the default Nouveau kernel driver" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo "# -----------------------------------------" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo "blacklist nouveau" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo "blacklist lbm-nouveau" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo "options nouveau modeset=0" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo "alias nouveau off" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
echo "alias lbm-nouveau off" | sudo tee -a /etc/modprobe.d/blacklist-nouveau.conf
```
To list the contents of the /etc/modprobe.d/blacklist-nouveau.conf file, issue the following command:
```
cat /etc/modprobe.d/blacklist-nouveau.conf
```
Step-5: Disable the Kernel mode setting (KMS) by issuing this command:
```
echo "options nouveau modeset=0" | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
```
To list the contents of the /etc/modprobe.d/nouveau-kms.conf file, issue the following command:
```
cat /etc/modprobe.d/nouveau-kms.conf
```
Step-6: Enter the following linux command to regenerate the kernel initramfs:
```
sudo update-initramfs -u
```
Step-7: Reboot the system.

Step-8: After the reboot you need to exit the X Server, for that we need to stop LightDM, press Ctrl+Alt+F1 to open up a console screen, log in with your user and password, after that:
```
sudo service lightdm stop
```
Step-9: Now install your Nvidia driver:
```
sudo apt-get install nvidia-VERSION
```
Note: The value of nvidia-VERSION could be nvidia-410, nvidia-412, nvidia-418, and so on, but you must be careful in locating correct Nvidia Display Driver. Ignoring this, may result in blank screen upon reboot.

Step-10: Reboot the system.

Step-11: To show which loadable kernel modules are currently loaded, issue the following command:
```
lsmod | grep nvidia
```
If there is an output, then the installation of nvidia is successful!

Step-12: Now issue the following command to know which display driver is loaded:
```
sudo lshw -c video | grep 'configuration'
```
*[refer link](https://askubuntu.com/questions/1129516/black-screen-at-boot-after-nvidia-driver-installation-on-ubuntu-18-04-2-lts)*

#### login loop error:
- Using `Ctrl + Alt + F2` to login commandline mode
- Then remove nvidia: `sudo apt remove nvidia*`

#### black screen boot with cursor
*Cannot login command line mode by `Ctrl + Alt + Fn`*

- Using live USB ubuntu
- First, I had to use my Ubuntu installation CD, and chose "Try Ubuntu".

- Next I logged into a terminal session, and have to remount my system partition (ie: /dev/sda1 is where I installed Ubuntu).
```
sudo mount /dev/sda1     /mnt
sudo mount --bind /dev  /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys  /mnt/sys
sudo chroot /mnt
```
- This mounts everything I need to run apt-get against my hard drive, rather than the non-persistent Ubuntu running in RAM.

- Now I just have to clobber Nvidia drivers, so I can get my system booting again.

- Remove existing drivers

```
sudo apt-get remove nvidia*
sudo apt-get purge nvidia*
```
- Housekeeping
```
sudo apt-get clean
sudo apt-get autoclean
```
- Handle any errors to due incomplete apt-get operations
```
sudo dpkg --configure -a
sudo apt-get update
sudo apt-get upgrade
```
- Remove xorg/X11/XFree86 references to graphics drivers
```
sudo rm /etc/X11/xorg.conf
sudo apt-get install ubuntu-desktop
```
- Clean up and unmount everything
```
exit
sudo umount /mnt/sys
sudo umount /mnt/proc
sudo umount /mnt/dev
sudo umount /mnt
exit
```
Now, I will NOT re-install the drivers just yet. Reboot the system, and eject the liveCD. I am now able to log into my existing Ubuntu installation.

*[refer link](https://superuser.com/questions/651596/ubuntu-12-04-lts-blinking-cursor-and-cannot-start-after-nvidia-driver-upgrade/651597#651597)*

-----------
# Ubuntu (16.04)
## Một số thủ thuật 

1. Đưa về launcher mặc định ban đầu
```
unity --reset-icons
```
[Tham khảo](http://ubuntuhandbook.org/index.php/2014/04/reset-unity-and-compiz-settings-in-ubuntu-14-04/)

2. Đưa lancher xuống dưới màn hình
```
gsettings set com.canonical.Unity.Launcher launcher-position Bottom
```
Thay `Bottom` bằng `Left` nếu muốn đưa về cạnh trái

[Tham khảo](https://www.howtogeek.com/251616/how-to-move-the-unity-desktops-launcher-to-the-bottom-of-your-screen-on-ubuntu-16.04/)

3. Sử dụng nhiều card mạng trên cùng 1 máy tính
Mình sử dụng 2 card mạng: 1 để thông với internet bên ngoài, 1 để kết nối với mạch zcu.  
Đặt ip tĩnh cho 2 mạng với 2 defaul gateway khác nhau.  
Dùng `ip route` để xem bảng route. Máy sẽ ưu tiên chọn route nào có `metric` nhỏ hơn trước. Ví dụ mình muốn ưu tiên gateway để thông lên internet thì dùng lệnh `sudo ifmetric enx00ea4c6d0753 200` để thiết lập metric cho card mạng enx00ea4c6d0753 thành 200.  
Một số lệnh: `ip route`, `traceroute 8.8.8.8` ...  
Tham khảo:
    https://superuser.com/questions/331720/how-do-i-set-the-priority-of-network-connections-in-ubuntu

4. Export .md thành pdf trên vs code

5. Cài đặt máy ubuntu làm server
    - Cài ssh server: 
      - `apt-get install openssh-server`
      - `systemctl enable ssh`
      - `systemctl start ssh`
      - [link tham khao](https://www.cyberciti.biz/faq/ubuntu-linux-install-openssh-server/)
    - Đổi port ssh server:
      - `sudo nano /etc/ssh/sshd_config`
      - Tìm kiếm dòng `port 22`, đổi port số 22 thành số khác (2247)
    - allow firewall: 
      - `sudo ufw allow 22/tcp`
    - setup ip tĩnh
    - reboot máy

6. Mount disk on start: 

    Tham khảo: https://csetutorials.com/auto-mount-ntfs-partitions-startup-ubuntu-linux.html

## Một số lỗi
- Lỗi "log in loop ubuntu" (boot đến đoạn log in, sau khi log in thì bị màn hình đen rồi sau đó lại trở về màn hình login)
    - `Ctrl + Alt + F1`, sau đó login vào....
### Lỗi network driver sau khi cài ubuntu trên máy bàn
Download driver here:
https://downloadcenter.intel.com/dow...?product=71307

Extract & cd into e1000e-3.3.5.3/src

Edit nvm.c, function e1000e_validate_nvm_checksum_generic (line 563) to (thêm cặp /*...*/):

Code:
```
s32 e1000e_validate_nvm_checksum_generic(struct e1000_hw *hw)
{
    /*
    s32 ret_val;
    u16 checksum = 0;
    u16 i, nvm_data;


    for (i = 0; i < (NVM_CHECKSUM_REG + 1); i++) {
        ret_val = e1000_read_nvm(hw, i, 1, &nvm_data);
        if (ret_val) {
            e_dbg("NVM Read Error\n");
            return ret_val;
        }
        checksum += nvm_data;
    }


    if (checksum != (u16)NVM_SUM) {
        e_dbg("NVM Checksum Invalid\n");
        return -E1000_ERR_NVM;
    }
    */


    return 0;
}
```

Build & install
Code:
```
make


sudo rmmod e1000e
sudo make install
sudo modprobe e1000e
```
It should work now


Source: https://unix.stackexchange.com/a/295052/23506


P.S. Checked on:

% lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description: Ubuntu 16.04.2 LTS
Release: 16.04
Codename: xenial


% uname -a
Linux 4.4.0-66-generic #87-Ubuntu SMP Fri Mar 3 15:29:05 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

## Ubuntu (18.04)
1. Thay đổi độ sáng màn hình PC:
    - Xem list màn hình:
        ` xrandr --listmonitors`
    - Thiết lập độ sáng:
        ` xrandr --output HDMI-0 --brightness 0.7`

2. Enable workspace:
http://ubuntuhandbook.org/index.php/2019/01/get-2x2-grid-workspaces-ubuntu-18-04/
 
3. Shutdown/reboot:
- ubuntu 18.04.03 không tắt/restart được sau khi cài Nvidia driver.
reboot bằng: `sudo reboot -f`

4. Không thể tạo, xóa, sửa trong ổ cứng khác:
`sudo mount -o remount,rw /dev/sda1`

## Command line
- `top -d 1`: resourse monitor

## install Kodi:
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:team-xbmc/ppa
sudo apt-get update
sudo apt-get install kodi
```

Add IPTV to TV live:
link iptv: https://vntvbox.com/list-tv-dung-cho-iptv-cap-nhat-lien-tuc-id3454.html

## Run Gui python on startup
link: https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-2-autostart

Tạo file .desktop để chạy chtrinh cần giao diện.

## Reset Ubuntu display
`dconf reset -f /`
Logout -> Log in

Nếu không được thì tham khảo thêm [tại đây](https://askubuntu.com/questions/362135/how-to-re-enable-tray-icons-for-applications-in-ubuntu/370654#370654)

## Find, Search in command line
- Find file name: `find path/to/folder -name "file"`
- Search file include text: 
  `grep -rnw path/to/folder -e 'text'`
