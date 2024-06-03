---
title: "SLAM Và Navigation"
date: 2024-05-08T06:48:02+07:00
draft: false
weight: 300
---

AMR sử dụng một số công nghệ chính là tạo bản đồ (SLAM Mapping), định vị và điều hướng.

## SLAM

SLAM - Simultaneous Localization And Mapping tạm dịch là công nghệ định vị và tạo bản đồ đồng thời. Bài toán SLAM xảy ra khi robot chưa có bản đồ của môi trường, cũng như vị trí của bản thân robot. Thay vào đó, robot chỉ có thông tin vè vị trí trước đo đã đo được (thông qua odometry chẳng hạn) và thông tin điều khiển (thông qua vận tốc). Thuật ngữ "Simultaneous Localization And Mapping" mô tả vấn đề: Trong SLAM, robot tạo được bản đồ môi trường xung quanh trong khi đồng thời thực hiện định vị vị trí của bản thân trong bản đồ đó.

SLAM được ứng dụng rất nhiều trong công nghệ robot tự hành hiện nay. Trong robot tự hành (AMR), SLAM chỉ ở bước tạo bản đồ (mapping). Còn lại phần lớn thời gian hoạt động về sau nó chỉ đơn thuần là hoạt động định vị dựa trên bản đồ có sẵn.

Dựa vào nguồn dữ liệu đầu vào chính, có thể chia làm visual SLAM dành cho phương pháp SLAM từ hình ảnh camera, lidar SLAM từ dữ liệu LiDAR scan và multi-sensor SLAM là phương pháp SLAM phối hợp nhiều loại cảm biến. Trong dự án này của tôi sử dụng LiDAR làm cảm biến chính, nên đó là LiDAR SLAM.

Trong cộng đồng sử dụng ROS, có 2 phương pháp SLAM chính đã được "chính thức" hóa trong các package của ROS đó là Gmapping (trong ROS 1) và mới nhất là slam_toolbox (trong ROS 2). Một số thuật toán SLAM khác có thể kể đến như EKF SLAM, Hecto SLAM, Karto SLAM, Catorgrapher...

## Định vị - Localization

Định vị là bài toán xác định vị trí của robot trong một môi trường đã có sẵn bản đồ. Bằng cách sử dụng một số cảm biến như LiDAR hoặc camera. Phần lớn thời gian khi AMR làm việc sẽ hoạt động ở bài toán định vị.

Bằng cách sử dụng các dữ liệu từ encoder, từ sensor (LiDAR, Camera...), robot sẽ quy chiếu vào bản đồ có sẵn để xác định vị trí của chính nó trong bản đồ. Cung cấp dữ liệu vị trí cho phần điều hướng điều khiển robot di chuyển tới các điểm theo yêu cầu cũng như di chuyển bám theo một quỹ đạo nào đó được định nghĩa từ trước.

Trong ROS 1, phổ biến nhất là thuật toán định vị dựa trên thuật toán Advance Monte-Carlo Localization - amcl. Thuật toán này dựa trên bộ lọc phân bố xác suất. Thuật toán này cũng được cung cấp trong ROS 2. Nó là thuật toán khá nhẹ, nhưng có một số nhược điểm là dễ bị nhầm lẫn vị trí và không quá ổn định.

Trong ROS 2, bản thân gói chương trình slam_toolbox cũng cung cấp một chế độ dành riêng cho bài toán định vị, nó hoạt động bằng cách tạo ra các bản đồ con (gọi là submap) tại thời điểm lân cận hiện tại (tức là từ n node gần nhất), tự định vị vị trí AMR trong bản đồ đó (là bài toán SLAM). Đồng thời, bản đồ con đó sẽ được so khớp bằng các bài toán tối ưu để tìm vị trí trong bản đồ gốc đã tạo trước đó. Từ đó tính ra được vị trí của robot trong bản đồ môi trường. Phương pháp này có nhiều ưu điểm hơn phương pháp amcl trước đó, tuy nhiên nó cũng khá nặng. Tùy vào điều kiện (năng lực tính toán của máy tính, chi phí) và yêu cầu độ ổn định của định vị sẽ cân nhắc lựa chọn phương pháp phù hợp.

## Điều hướng - Navigation

Điều hướng robot là bài toán xác định kế hoạch lộ trình di chuyển của robot trong một môi trường. Ví dụ cần di chuyển robot từ điểm A tới điểm B trong môi trường đã biết. Nhiệm vụ của điều hướng là tìm đường đi từ A tới B trên bản đồ. Trong bài toán điều hướng robot tới một vị trí đích, đầu tiên dựa vào bản đồ để xác định đường đi tới điểm đích - gọi là global planner. Sau đó dựa vào trạng thái hiện tại của robot để xác định một đường đi khả dĩ, điều khiển robot bám theo đường đi đã hoạch định ở trước, đồng thời có thể xử lý tránh vật cản trên đường đi - gọi là local planner.

Có nhiều thuật toán đã được phát triển thành các gói phần mềm trong ROS từ ROS 1 tới ROS 2. Trong ROS 2, chúng ta sẽ làm quen với [navigation 2](https://github.com/ros-navigation/navigation2) cung cấp đầy đủ công cụ để điều hướng robot.

![navigation_with_recovery_behaviours.gif](/images/1.nen-tang/navigation_with_recovery_behaviours.gif)

Tham khảo: https://docs.nav2.org/getting_started/index.html

