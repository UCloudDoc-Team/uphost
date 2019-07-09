# 性能测试

{{indexmenu_n>4}}

硬盘性能指标

**顺序读写** （吞吐量，常用单位为MB/s）：文件在硬盘上存储位置是连续的。

适用场景：大文件拷贝（比如视频音乐）。速度即使很高，对数据库性能也没有参考价值。

**4K随机读写IOPS** （常用单位为次）

**4K随机读写延迟** （常用单位为μs）

在硬盘上随机位置读写数据，每次4KB。

适用场景：操作系统运行、软件运行、数据库。

以下测试比较了数据库型物理云主机（SAS磁盘）与SSD型云主机在Raid10与Raid5下的三项性能指标。

## 测试数据

### 测试1. 顺序读/写512K

![image](/images/seq512k.png)

### 测试2. 随机读/写 4K (IOPS)

![image](/images/rand4k.png)

### 测试3. 随机读/写 4K (IO延迟)

![image](/images/rand4kdelay.png)

## 测试详情

> 工具：fio
> 
> 官方网站：
> 
> <http://freecode.com/projects/fio>
> 
> <http://brick.kernel.dk/snaps/>

**注意:
性能测试建议直接通过写裸盘的方式进行测试，会得到较为真实的数据。但直接测试裸盘会破坏文件系统结构，导致数据丢失，请在测试前确认磁盘中数据已备份。**

## 测试命令

512K顺序写、读

```
/usr/bin/fio -filename=/dev/sdb -direct=1 -iodepth 64 -thread -rw=write  -ioengine=libaio -bs=512K  -numjobs=8 -runtime=1200 -group_reporting -name=test
/usr/bin/fio -filename=/dev/sdb -direct=1 -iodepth 64 -thread -rw=read  -ioengine=libaio -bs=512K  -numjobs=1 -runtime=120 -group_reporting -name=test
```

4K随机写、读

```
/usr/bin/fio -filename=/dev/sdb -direct=1 -iodepth 64 -thread -rw=randwrite  -ioengine=libaio -bs=4K  -numjobs=8 -runtime=120 -group_reporting -name=test
/usr/bin/fio -filename=/dev/sdb -direct=1 -iodepth 64 -thread -rw=randread -ioengine=libaio -bs=4K -numjobs=8 -runtime=120 -group_reporting -name=test
```

4K随机写、读延时

```
/usr/bin/fio -filename=/dev/sdb -direct=1 -iodepth 1 -thread -rw=randwrite  -ioengine=libaio -bs=4K  -numjobs=1 -runtime=120 -group_reporting -name=test
/usr/bin/fio -filename=/dev/sdb -direct=1 -iodepth 1 -thread -rw=randread  -ioengine=libaio -bs=4K  -numjobs=1 -runtime=120 -group_reporting -name=test
```
