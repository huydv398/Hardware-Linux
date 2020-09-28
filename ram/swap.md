# Swap trên hệ thống Linux

Có hai loại bộ nhớ cơ bản 
* RAM - Random Access Memory: Được sử dụng để lưu trữ dữ liệu và chương trình trong khi chúng được máy tính sử dụng. Máy tính không thể sử dụng các chương trình và dữ liệu trừ khi chúng được lưu trữ trong RAM. Dữ liệu được lưu trữ trong RAM sẽ bị mất nếu máy tính bị tắt

Ổ cứng là phương tiện được sử dụng để lưu trữ lâu dài các dữ liệu và chương trình. Dữ liệu được lưu trên đĩa vẫn còn khi mà nguồn điện được rút khỏi máy tính.

CPU (central processing unit) không thể truy cập trực tiếp vào các chương trình và dữ liệu trên ổ cứng; Nó phải được sao chép vào RAM trước và đó là nơi CPU có thể truy cập các tập lệnh của nó và dữ liệu được vận hành theo các lệnh đó. Trong quá trình khởi động, máy tính sao chép các chương trình hệ điều hành cụ thể, chẳng hạn như kernel và init hoặc systemd, và dữ liệu từ ổ cứng vào RAM, nơi nó được truy cập trực tiếp bởi bộ xử lý của máy tính, CPU.

Loại bộ nhớ thứ hai là SWAP Space.
## Swap là gì?
### Chức năng
* Dùng không gian của disk hoán đổi làm không gian cho bộ nhớ RAM khi RAM đầy và cần thêm dung lượng.

Ví dụ: Giả sử hệ thống có 4GB RAM. Nếu bạn khởi động chương trình và chưa lấp đầy bộ nhớ RAM; thì khi đó swap chưa cần hoạt động. Nhưng việc sử dụng và phát triển nhiều hơn và cộng với mọi thứ đang hoạt động trên máy dẫn đến việc sử dụng hế dung lượng RAM. Nếu không có SWAP thì bạn sẽ phải ngưng làm việc và đóng một số chương trình để giải phóng RAM.

* Kernel sử dụng một chương trình để quản lý bộ nhớ để phát hiện các blocks, pages của bộ nhớ trong đó nội không được sử dụng gần đây. Chương trình quản lý bộ nhớ hoán đổi đủ các trang bộ nhớ được sử dụng không thường xuyên này sang một phân bùng đặc biệt trên ổ cúng được chỉ định cụ thể cho "**Phân trang- Paging**" hoặc swapping. Điều này giải phóng RAM và tạo chỗ cho nhiều dữ liệu hơn được nhập vào.
Những trang của bộ nhớ đã hoán đổi sang ổ cứng được theo dõi bởi mã quản lý memery của kernel và có thể được phân trang trở lại RAM nếu chúng cần.

Tổng dung lượng bộ nhớ- Total trong một máy tính Linux là RAM cộng với Swap và được gọi là virtual memory- bộ nhớ ảo.

## Các loại Linux Swap 
Linux cung cấp 2 loại hoán đổi. Theo mặc định, hầu hết các bản cài đặt Linux đều tạo swap Partition, cũng có thể sử dụng tệp được cấu hình đặc biệt làm Swap File. Swap Partition là một vùng disk tiêu chuẩn được chỉ định làm Swap space bằng lệnh mkswap. Đây chỉ là một tệp thông thường được tạo và phân bổ trước theo kích thước được chỉ định. Sau đó, Lệnh `mkswap` được chạy để cáu hình nó làm swap. 

* Swap file
* Swap partition 

Nên sử dụng Swap partition, swap file chỉ sử dụng trong trường hợp thực sự cần thiết.

## Thrashing
Thrashing có thể xảy ra khi tổng bộ nhớ ảo, cả RAM và swap, gần đầy. Hệ thống dành rất nhiều thời gian để phân trang các paging blocks of memory giữa swap và ram và trở lại ít thời đó cho công việc thực sự. Các biểu hiện điển hình của điều này rất rõ rằng: Hệ thống trở nên chậm hoặc hoàn toàn không phản hồi và đèn báo hoạt động của ổ cứng gần như sáng liên tục. 

Nếu bạn có thể quản lý để đưa ra lệnh như `free` hiển thị tải CPU và memory đang sử dụng, bạn sẽ thấy rằng CPU Load rất cao, có thể gấp 30-40 lần số CPU cóe trong hệ thống. Một biểu hiển khác là cả RAN và Swap được lấp đầy hoàn toàn.

Sau thực tế, nhìn vào dữ liệu SAR(system activity report-báo cáo hoạt động của hệ thống) cũng có thể cho thấy các triệu chứng này. Tôi cài đặt 