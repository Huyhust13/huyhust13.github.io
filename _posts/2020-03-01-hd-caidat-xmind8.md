---
layout: post
title: Hướng dẫn cài đặt Xmind 8 trên Ubuntu
---
# Hướng dẫn cài đặt Xmind 8 trên Ubuntu
Chắc hẳn các bạn đã quen thuộc với công cụ mindmap, trên window có Mind Manager tuy mất phí nhưng bạn có thể dễ dàng "cờ rắc" được, còn trên Ubuntu thì tương đối hạn chế, có freemind (nhẹ, nhưng lại đơn giản quá, hơi thiếu chức năng), XMind Zen thì mất phí. Mình chọn dùng Xmind 8 vì nó "vừa đủ" chức năng của một bản free.
## Cài Java
[link hướng dẫn](https://www.guru99.com/how-to-install-java-on-ubuntu.html)
Các bạn lưu ý chọn phiên bản java8 (Vì mình cài bản 12 thì không chạy được)

## Cài đặt XMind
1. Download  
Các bạn vào [link này](https://www.xmind.net/download/xmind8) để down bản xmind 8 dùng cho ubuntu về (đây là file .zip).   
2. Giải Nén  
```
unzip <ten_file_xmind>.zip -d xmind
```
3. Move thư mục xmind tới /opt/  
```
sudo mv xmind /opt/
```  
4. Cấp quyền ghi cho thư mục configuration  
```
sudo chmod a+x /opt/xind/XMind\_amd64/configuration
```
5. Sửa file config /opt/xmind/XMind_amd64/XMind.ini  

	Dòng thứ 2: `Sửa ./configuration` thành `/opt/xmind/XMind_amd64/configuration` 
	Tạo thư mục `workspace/` trong `/opt/xmind/` và cấp quyền ghi cho nó
	Dòng thứ 4: sửa ../workspace thành `/opt/xmind/workspace`
	Dòng 6 & 8: Sửa ../plugins thành `/opt/xmind/plugins`  

6. Tạo file xmind.desktop
```
sudo gedit /usr/share/applications/xmind.desktop
 ```
Paste nội dung như sau:
```
[Desktop Entry]
Comment=Create and share mind maps
Exec=/opt/xmind/XMind_amd64/XMind  %F
Name=XMind
Terminal=false
Type=Application
Categories=Office
Icon=/opt/xmind/xmind.jpg
```
*Trước đó, bạn hãy lên gg search lấy 1 icon xmind [có thể dùng ảnh này](https://www.google.com/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&ved=2ahUKEwib_fmzjd7iAhWXdHAKHc37B80QjRx6BAgBEAU&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FXMind&psig=AOvVaw18eiz7bYiUkYeCGQYO1zog&ust=1560228277091650), lưu về trong thư mục `/opt/xmind/` hoặc bất kì đâu rồi dẫn link tới trong lệnh `Icon=...`*.

Cấp quyền thực thi cho file xmind.desktop  
```
sudo chmod +x /usr/share/applications/xmind.desktop
```

Kiểm tra cài đặt thành công hay không, các bạn vào launcher, tìm đến xmind và bật nó lên...

![alt text](/img/xmind_1.png)
Done!