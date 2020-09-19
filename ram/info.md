# Ram 
Hệ thống Linux dùng RAM của hệ thống.

* Linux đang mượn bộ nhớ không sử dụng để làm Disk Caching. Điều này có vẻ như sắp hết bộ nhớ nhưng không phải vậy.

![huydv](/ram/image/Screenshot_1.png)
* Disk caching giúp hệ thống nhanh hơn và phản hồi nhanh hơn. Không có nhược điểm. Nó không lấy bộ nhớ của các ứng dụng theo bất kỳ cách nào.
* Nếu muốn chạy nhiều ứng dụng hơn; Nếu ứng dụng của bạn muốn thêm bộ Memory , chúng ta chỉ lấy lại một phần mà bộ nhớ disk cache đã mượn. Disk cache luôn có thể được trả lại cho ứng dụng ngay lập tức
* Có cần nâng cấp Swap ?
    * Disk caching chỉ mượn ram mà các ứng dụng hiện tại không cần. Nó sẽ không sử dụng Swap. Nếu các ứng dụng cần thêm bộ nhớ, họ chỉ cần lấy lại từ bộ nhớ disk cache. họ sẽ không chạy Swap.
* Ngăn chặn Linux lấy Ram. Bạn không thể tắt disk caching. Lý do mọi người muốn tắt Disk caching là họ nghĩ rằng nó sẽ lấy đi bộ nhớ mà các ứng dụng đang dùng, điều này thì không. Disk cache giúp các ứng dụng tải nhanh hơn và chạy mượt mà hơn, nhưng không bao giờ nó lấy bộ nhớ của các ứng dụng đang sử dụng. Không có lý do gì để vô hiệu hóa nó.
## Làm cách nào để thực sự biết còn bao nhiêu ram trống?
Để xem còn bao nhiêu ram được sử dụng, chạy lệnh `free -m` và nhìn cột `available`:
```
[root@srv1 ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:           3773         213        3316          11         243        3304
Swap:             0           0           0

```
* `-m` được hiển thị dưới dạng `MiB`

## Dấu hiệu
Các dấu hiệu cảnh báo về tình trạng bộ nhớ thấp thực sự mà bạn có thể xem xét* 
* `available` (hoặc "`free + buff/cache") gần bằng 0
* `swap used` Tăng hoặc giao động
* `dmesg | grep oom-killer` hiển thị OutOfMemory-killer trên máy
## Kết luận

* Linux Mượn bộ nhớ ram không sử dụng để lưu và cache. Điều này làm cho người dùng hiểu lầm khi nhìn vào chỉ số của `free`.
* Bản chất điều này làm tăng tốc độ của linux
* Bất cứ khi nào ứng dụng cần thêm bộ nhớ. Nó sẽ hoàn trả ngay lập tức hoặc nó sẽ được hoàn trả lại sau khi hoàn thành chức năng.