在开始DMA传输时,主机向内存写入DMA命令块,向 DMA控制器写入该命令块的地址,启动I/O设备。然后,CPU继续其他工作,DMA控制器则继续下去直接操作内存总线,将地址放到总线上开始传输。当整个传输完成后,DMA 控制器中断 CPU。

![[Pasted image 20230916195618.png]]![[Pasted image 20230916195627.png]]
