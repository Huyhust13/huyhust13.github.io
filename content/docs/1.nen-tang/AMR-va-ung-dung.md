---
title: "AMR Và Ứng Dụng"
date: 2024-05-08T06:47:34+07:00
draft: false
weight: 100
---

## AMR là gì?

AMR - Viết tắt của **A**utonomous **M**obile **R**obot là kiểu robot dưới sự điều khiển của PLC hoặc PC (phần lớn là PC), có thể định vị và điều hướng một cách độc lập mà không cần các hệ thống dẫn đường vật lý như băng từ, băng phản quang, QR, ... Đồng thời nó có khả năng tránh các vật cản cố định và di động trên đường đi.

AMR được trang bị nhiều loại cảm biến phức tạp, công nghệ máy học (machine learning) và trí thông minh nhân tạo và để tính toán đường đi tới các vị trí cần thiết trong môi trường hoạt động của nó.

AMR là một lĩnh vực khá non trẻ, tuy nhiên có rất nhiều hãng AMR trên Thế giới hiện nay. Có thể kể đến một vài hãng nổi tiếng như [MIR](https://mobile-industrial-robots.com/), [SEER Group](https://www.seer-group.com/)...

![MIR 250](/images/1.nen-tang/MiR250.png)

Tại Việt Nam, có một số công ty, phòng lab tại trường đại học bắt đầu tiếp cận, nghiên cứu và phát triển AMR nhờ sự phổ biến của nền tảng ROS vào nhũng năm 2017, 2018 như ở Bách Khoa Hà Nội, công ty [Meiko Automation](https://meiko-auto.vn/), [Esatech](https://smartagv.net/) và đặc biệt là Techvico - nơi tôi hiện tại đang làm việc.

  Với sự phổ biến của nền tảng ROS, và hiện nay mới nhất là ROS2, chúng ta có thể nhanh chóng triển khai một AMR trong thời gian ngắn hơn nhờ dựa trên các mã nguồn mở. Chúng ta sẽ tìm hiểu xem ROS là gì ở phần tiếp theo.

## AMR được ứng dụng ở đâu?

AMR hiện nay trên Thế Giới và tại Việt Nam có ứng dụng khá rộng rãi.

1. Trong Công nghiệp
- Vận chuyển trong kho hàng
- Vận chuyển cung cấp vật liệu trong các dây chuyền sản xuất, lắp ráp
- Vận chuyển thành phẩm tới các khu vực đóng gói
...

2. Trong dịch vụ
- Robot tự hành thông minh có thể được trang bị thêm các công nghệ thông minh khác để phục vụ trong các nhà hàng, sân bay, khách sạn như các robot dịch vụ. Vừa di chuyển tự động, vừa thực hiện các tác vụ khác như theo dõi, quan sát, lễ tân...

3. Trong y tế
- Trong y tế, đặc biệt là trong đại dịch Covid 19 vừa qua, robot tự hành đã có một số ứng dụng quan trọng giúp phun thuốc khử khuẩn, vận chuyển đồ đạc, thiết bị, thức ăn qua các vùng cách ly.

Ngoài ra còn có nhiều ứng dụng khác có thể mở rộng không gian làm việc cho con người.