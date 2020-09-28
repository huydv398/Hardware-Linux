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

Sau thực tế, nhìn vào dữ liệu SAR(system activity report-báo cáo hoạt động của hệ thống) cũng có thể cho thấy các triệu chứng này. Tôi cài đặt SAR trên mọi hệ thống mà tôi làm việc và sử dụng nó để phân tích sau sửa chữa

## Đặt dung lượng Swap cho phù hợp

|Total RAM|Server|Hệ thống Desktop|
|-|-|-|
|< 2GB|2 x RAM|3 x RAM|
|2GB < RAM < 8GB|= RAM|2 x RAM|
|8GB < RAM < 64GB|RAM /2 |RAM x 1,5|
|64GB < RAM|0.5 x RAM|Không khuyến khích|

## Cấu hình Swap 
### 1. SWAP Partition 
* Xác định partition nào sẽ lấy làm swap

`fdisk -l`
### 2. Swap File
1. Tạo một tệp sẽ được sử dụng để làm Swap:

`fallocate -l 1G /swapfile`

Nếu `fallocate` chưa được cài đặt hoặc neesy banjnhanaj được thông báo lỗi `fallocate` thì bạn có thể sử dụng lệnh sau để tạo một Swap File:

`dd if=/dev/zero of=/swapfile bs=1024 count=1048576`

2. Phân quyền. Chỉ người dùng root mới có thể viết và đọc swap fiel

`chmod 600 /swapfile`

3. Sử dụng tiện ích `mkswap` để thiết lập file làm vùng Linux Swap:

`mkswap /swapfile`

4. Cho phép dùng Swap

`swapon /swapfile`

Để cài đặt luôn được thực hiện khi restart ,

` echo "/swapfile swap swap defaults 0 0" >> /etc/fstab`

5. Để xác minh rằng swap đang hoạt động, sử dụng lệnh `swapon` hoặc `free`
```
[root@localhost ~]# swapon --show
NAME       TYPE       SIZE USED PRIO
/dev/dm-1  partition    2G 520K   -1
/swapfile1 file      1024M   0B   -2
```

hoặc 

```
free -h

              total        used        free      shared  buff/cache   available
Mem:           974M        135M         62M        7.4M        777M        654M
Swap:          3.0G        520K        3.0G
```
## Điều chỉnh giá trị Swappiness 
Swappiness là một thuộc tính hạt nhân Linux xác định tần suất hệ thống sẽ sử dụng swap. Swappiness có thể có giá trị từ 0 đến 100. Giá trị thát sẽ khiến kernel có gắng tránh hoán đổi bất cứ khi nào có thể, trong khi giá trị cao hơn sẽ khiến kernel sử dụng swap nhiều hơn.

Giá trị swap mặc định là 30-60. Kiểm tra giá trị Swappiness hiện tại:

```
cat /proc/sys/vm/swappiness

30
```

Để thay đổi giá trị swap là 10:

`sysctl vm.swappiness=10`

Giá trị Swappiness tối ưu phụ thuộc vào khối lượng công việc hệ thống của bạn và các bộ nhớ đang được sử dụng. Bạn nên điều chỉnh thông số này theo từng bước nhở để tìm ra giá trị tối ưu.

## Cách xóa swap file
1. Hủy kích hoạt:

`swapoff -v /swapfile`
2. Xóa mục nhập tên khỏi tệp `/etc/fstab`
3. Xóa tệp

`rm /swapfile`

### vfs_cache_pressure
Để xem được `vfs_cache_pressure` chúng ta chỉ cần dùng command :

`cat /proc/sys/vm/vfs_cache_pressure`

Tham số `vfs_cache_pressure` là thâm số ảnh hưởng trực tiếp dến việc lưu trữ các mục siêu dữ liệu của hệ thống (filesystem metedata). Thực chất thì tham số này quyết định đến việc kernel lấy lại các memory đang được sử dụng cho vfs_cache

Default giá trị của tham số này là 100. việc tăng giá trị này sẽ làm tăng tốc độ lấy lại bộ nhớ VFS. Để swap hoạt động tốt nhất  chúng ta nên để nó về giá trị 50.

`echo "vm.vfs_cache_pressure=50 >> /etc/sysctl.conf`
Link tham khảo:
* https://viblo.asia/p/swap-eW65GWBJ5DO
* https://linuxize.com/post/create-a-linux-swap-file/