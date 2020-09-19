# Thống kê chung
```
[root@srv1 ~]# cat /proc/meminfo
MemTotal:        3863568 kB
MemFree:         3396032 kB
MemAvailable:    3383712 kB
Buffers:            2112 kB
Cached:           189352 kB
SwapCached:            0 kB
Active:           186944 kB
Inactive:         132364 kB
Active(anon):     128732 kB
Inactive(anon):    11092 kB
Active(file):      58212 kB
Inactive(file):   121272 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 4 kB
Writeback:             0 kB
AnonPages:        127988 kB
Mapped:            46188 kB
Shmem:             11980 kB
Slab:              57856 kB
SReclaimable:      25632 kB
SUnreclaim:        32224 kB
KernelStack:        5072 kB
PageTables:         7804 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     1931784 kB
Committed_AS:    1062220 kB
VmallocTotal:   34359738367 kB
VmallocUsed:      182476 kB
VmallocChunk:   34359310332 kB
HardwareCorrupted:     0 kB
AnonHugePages:     18432 kB
CmaTotal:              0 kB
CmaFree:               0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
DirectMap4k:       87872 kB
DirectMap2M:     4106240 kB
DirectMap1G:     2097152 kB

```
## Thống kê chung
* MemTotal: tổng dụng lượng ram có thể sử dụng, tính bằng kibibyte, là ram vật lý trừ đi một số bit dự trữ  và mã nhị phân kernel
* MemFree: Dung lượng ram vật lý được hệ thống không sử dụng
* MemAvailable: Ước tính dung lượng bộ nhớ có sẵn để khởi động các ứng dụng mới  mà không cần hoán đổi. Được tính toán từ MemFree, SReclaimable,... Ước tính có tính đến việc hệ thống cần một số bộ nhớ cache để hoạt động tốt. Tác động của các yếu tố đó sẽ khác nhau giữa các hệ thống.
* Buffers: Lưu trữ tạm thời các khối raw disk(không quá quan tâm 20M hoặc hơn)
* Cached: được sử dụng làm bộ nhớ đệm