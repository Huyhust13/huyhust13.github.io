---
title: "Cơ Bản Về ROS"
date: 2024-05-08T06:46:59+07:00
draft: false
weight: 200
---

## ROS là gì?

ROS - **R**obot **O**perating **S**ystem là một bộ phần mềm mã nguồn mở dùng cho việc phát triển các ứng dụng Robotics. ROS cung cấp một nền tảng phần mềm tiêu chuẩn để các nhà phát triển có thể nhanh chóng từ khâu nghiên cứu, chế tạo thử sang triển khai và sản phẩm một cách dễ dàng và nhanh chóng.

> *Don't reinvent the wheel. Create something new and do it faster and better by building on ROS. - From ros.org.*

ROS là một tập hợp gồm rất nhiều thư viện và công cụ để xây dựng các ứng dụng robot. Từ các driver (các chương trình làm việc với các thiết bị phần cứng), từ các giải thuật tiên tiến, cập nhật nhất cho tới các công cụ cực kỳ hữu ích cho các nhà phát triển.

ROS được giới thiệu lần đầu từ nắm 2007, đã có rất nhiều thay đổi trong robotics và cộng đồng ROS. Và ROS 2 ra đời để đáp ứng và thích nghi với các thay đổi đó.

## Cài đặt ROS

Hiện nay ROS2 có thể chạy được trên các hệ điều hành Linux, Window, MacOS. Tuy nhiên, để đạt được mức độ tương thích và cập nhật nhất, tôi vẫn khuyên các bạn sử dụng ROS trên hệ điều hành Ubuntu.

Tương tự như ROS1, ROS2 có các phiên bản khác nhau, tương thích với các phiên bản hệ điều hành khác nhau. Tuy nhiên, ở ROS1 mỗi phiên bản ROS chỉ chạy được trên một phiên bản Ubuntu nhất định. Còn ở ROS2 thì một phiên bản ROS có thể chạy được được trên nhiều hơn 1 phiên bản Ubuntu. Hiện tại tôi đang sử dụng ROS2 Humble trên Ubuntu 22.04 cho dự án Hbot này. Vậy nên các hướng dẫn trong blog này sẽ được viết trên nền tảng ROS2 Humble và chạy trên Ubuntu 22.04.

Hướng dẫn cài đặt ROS 2: [Tham khảo tại đây](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)

## Hướng dẫn cơ bản

### Cấu hình môi trường

Trong ROS, chúng ta có thuật ngữ `workspace` dùng để chỉ vị trí thư mục bạn đang phát triển các gói ROS 2. Trong quá trình phát triển ROS 2, bạn có thể sẽ chạy nhiều `workspace` cùng một lúc.

Việc chia các workspace khác nhau có thể vì mục đích gom các gói chương trình có liên quan với nhau thành từng workspace, hoặc cũng có thể sử dụng các workspace khác nhau để chạy trên nền tảng các phiên bản (distro) ROS 2 khác nhau. Điều này cho phép bạn cài đặt và sử dụng nhiều phiên bản ROS 2 trên cùng một máy tính.

Bạn phải `source` tới các file cấu hình mỗi khi bật một shell mới để có thể sử dụng được các lệnh của ROS 2, tìm các packages của ROS 2.

Để dẫn xuất tới file cấu hình môi trường ROS, mỗi khi mở shell mới bạn chạy lệnh sau:

```
source /opt/ros/humble/setup.bash
```

> Thay thế `humble` bằng tên phiên bản ROS mà bạn đang dùng.
> Thay thế `.bash` bằng `.zsh` nếu bạn dùng zsh cho shell.

Việc trên cũng có thể được thực hiện một cách tự động bằng cách thêm dòng source trên vào file `~/.bashrc` (đối với bash) hoặc `~/.zshrc` (đối với zsh).

Sử dụng lệnh sau để kiểm tra phiên bản ROS đang dùng:

```
printenv | grep -i ROS
```

Kết quả sẽ tương tự như sau:

```
ROS_VERSION=2
ROS_PYTHON_VERSION=3
ROS_DISTRO=humble
```

ROS 2 khác ROS 1 ở một điểm cơ bản đó là trong ROS 2 mặc định các thông tin của ROS sẽ được public ra mạng local. Cụ thể là nếu 2 máy cùng sử dụng chung một mạng nội bộ, mặc định khi một máy chạy ROS 2 và publish một số thông tin, máy tính còn lại sẽ có thể truy cập vào các topic đấy.

Để quản lý được việc đó, chúng ta sử dụng biến môi trường `ROS_DOMAIN_ID`, khi đó các máy có cùng `ROS_DOMAIN_ID` trong cùng một lớp mạng internet mới có thể giao tiếp được với nhau. Hoặc khai báo biến môi trường `ROS_LOCALHOST_ONLY` để giới hạn ROS 2 chỉ có thể giao tiếp trong nội bộ localhost.

### Một số thuật ngữ và khái niệm cơ bản trong ROS 2

#### Node

Node là một thành phần cơ bản nhất trong ROS, mỗi node thường được thiết kế để đảm nhận cho một mục đích. Ví dụ node để điều khiển bánh xe, node đề tính toán odometry... Mỗi node có thể gửi, nhận dữ liệu từ các node khác trong cùng một môi trường ROS thông qua `topic`, `service`, `action` hay `parameter`.
Hình dưới đây mô tả đơn giản cách cách node giao tiếp với nhau.

![NodesTopicAndService.gif](/images/1.nen-tang/Nodes-TopicandService.gif)

Một hệ thống robotics hoàn chỉnh chứa nhiều hoặc rất nhiều node hoạt động cho cùng một mục tiêu. Trong ROS2, mỗi một file thực thi có thể chứa một hoặc nhiều node (Khác với ROS 1).

Một số lệnh với `node` như:

- Để chạy một node từ terminal:
```
ros2 run <package_name> <executable_name>
```

- Để liệt kê danh sách các node đang chạy:
```
ros2 node list
```

- Để xem thông tin của node
```
ros2 node info /<node_name>
```

#### Topic

`Topic` là thuật ngữ chỉ một loại dữ liệu được truyền từ các `node publisher` tới các `node subscriber`. Một loại topic có thể dùng để truyền dữ liệu giữa nhiều publisher và nhiều subscriber.

![Topic-MultiplePublisherandMultipleSubscriber.gif](/images/1.nen-tang/Topic-MultiplePublisherandMultipleSubscriber.gif)

Có thể sử dụng `rqt_graph` để hiển thị các node và các topic giao tiếp giữa các node đó trong một mạng ROS.

- Liệt kê danh sách các topic hiện có trong mạng ROS
```
ros2 topic list
```

- Hiển thị thông tin topic. Lệnh này sẽ trả về kiểu dữ liệu của topic và số node đang publish và đang subscribe vào topic đó.

```
ros2 topic info <Tên_topic>
```

- Chi tiết kiểu dữ liệu của topic, để xem chi tiết kiểu dữ liệu bên trong:
```
ros2 interface show <Tên_kiểu_message>
# Ví dụ:
ros2 interface show geometry_msgs/msg/Twist
# Kết quả:
# # This expresses velocity in free space broken into its linear and angular parts.
#
#    Vector3  linear
#            float64 x
#            float64 y
#            float64 z
#    Vector3  angular
#            float64 x
#            float64 y
#            float64 z
```

- Xem tần số publish của topic
```
ros2 topic hz <Tên_topic>
```

- Subscibe vào topic
```
ros2 topic echo /<tên_topic>
```

- Publish một topic từ terminal
```
ros2 topic pub <tên_topic> <tên_kiểu_message> '<dữ_liệu>'
```

#### Service

`Service` là một phương pháp giao tiếp khác giữa các node trong mạng lưới ROS. Các `service` dựa trên mô hình gọi và trả lời. Trong khi `topic` cho phép các node truyền dữ liệu liên tục với nhau, service chỉ cung cấp dữ liệu khi được yêu cầu bởi một client. Hình dưới mô tả cách service hoạt động.

![Service-MultipleServiceClient.gif](/images/1.nen-tang/Service-MultipleServiceClient.gif)

Một số câu lệnh đơn giản để làm việc với service trên terminal:

- `ros2 service list`: Liệt kê danh sách các service trong mạng ROS
- `ros2 service type <tên_service>`: Xem thông tin kiểu dữ liệu của service đó.
- `ros2 service call <tên_service> <tên_kiểu_request> '<dữ_liệu>'`: Gửi request đó cho service đó.

#### Parameters

Một `parameter` là một giá trị cấu hình của một node. Bạn có thể coi các `parameter` là các tham số cấu hình của một node. Một node có thể lưu các `parameter` với các kiểu dữ liệu int, float, boolean, string, list. Trong ROS 2, mỗi node duy trì các prameter của nó.

Một số lệnh CLI với parameter:

- `ros2 param list`: Liệt kê danh sách các parameter trong mạng ROS
- `ros2 param get <tên_node> <tên_parameter>`: Xem giá trị parameter đó
- `ros2 param set <tên_node> <tên_parameter> <giá_tri>`: Định giá trị parameter đó
- `ros2 param dump <tên_node> > <tên_file>`: Ghi giá trị các parameter của node ra file.
...

#### Action

Action cũng là một kiểu truyền thông dữ liệu giữa các node trong ROS 2, nó được dùng cho các nhiệm vụ có thời gian hoạt động dài. Action bao gồm 3 phần: mục tiêu (goal), phản hồi (feedback) và kết quả (result).

Action được xây dựng dựa trên topic và service. Cách hoạt động tương tự như service ngoại trừ việc action có thể hủy được.

Action sử dụng mô hình client-server, tương tự như mô hình publisher-subscriber. Một node `action client` gửi một mục tiêu (goal) tới node `action server`. Sau đó `action server` sau đó `action server` thực hiện nhiệm vụ để đạt được mục tiêu đó, trong quá trình thực hiện nhiệm vụ, nó liên tục cập nhật tình hình cho `action client` bằng cách gửi phản hồi (feedback) liên tục về cho `action client`, và khi đã đạt mục tiêu, nó gửi kết quả (result). Mô tả trực quan như hình dưới.

![Action-SingleActionClient.gif](/images/1.nen-tang/Action-SingleActionClient.gif)

Một số lệnh CLI với `action`:
- `ros2 action list` : Liệt kê danh sách các action trong mạng ROS
- `ros2 action info <tên_action>`: để hiển thị thông tin action đó, bao gồm client, server.
- `ros2 interface show <tên_kiểu_action>`: Xem chi tiết kiểu dữ liệu action đó
- `ros2 action send_goal <tên_action> <tên_kiểu_action> '<dữ_liệu>'`: Gửi request cho action.

Tóm lại, action tương tự service, cho phép thực thi một nhiệm vụ trong một thời gian dài hơn, cung cấp cơ chế phản hồi và có thể hủy được trong quá trình thực hiện. Trong điều hướng robot, một action điển hình là gửi một vị trí mục tiêu (goal) để ra lệnh cho robot di chuyển tới điểm mục tiêu, trong quá trình di chuyển robot liên tục cập nhật thông tin về cho `action client` và cuối cùng là bản tin kết quả khi đã đến được mục tiêu.

#### Một số công cụ hữu ích trong ROS

Có một số công cụ rất hữu ích trong quá trình phát triển các ứng dụng với ROS đó là Rviz, rqt_graph, rqt_console, ...

Trong khuôn khổ tài liệu này mình sẽ không đi chi tiết hướng dẫn ROS. Trên đây là các kiến thức cơ bản để giới thiệu các thành phần cơ bản nhất của ROS. Để chi tiết hơn các bạn có thể xem ở link tài liệu tham khảo bên dưới, hoặc có thể liên hệ với mình trong trường hợp gặp các vấn đề thắc mắc.

## Tham khảo
[1] https://docs.ros.org/