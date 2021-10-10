# commonly_used_ip_core
学习和开发过程中经常使用的ip core(只包含HDL代码)
## xdma_irq_ctrl
xdma的中断控制ip core，使用标准的AXI4-Lite协议，实现xdma的中断产生和清除。<br>
### Block Diagram
![bd](https://github.com/XiaogegeChen/commonly_used_ip_core/blob/main/xdma_irq_ctrl/pic/bd.png)

```S_AXI```端口用于控制中断的写入和清除，```xdma_user_irq_req```端口用于提供中断信号给xdma，连接再xdma的```usr_irq_req```端口。

### Parameters
![params](https://github.com/XiaogegeChen/commonly_used_ip_core/blob/main/xdma_irq_ctrl/pic/cfg.png)

```Intr Count```用于配置中断数量，支持的范围为```[1...16]```

### System Design Exampel
![sys_bd](https://github.com/XiaogegeChen/commonly_used_ip_core/blob/main/xdma_irq_ctrl/pic/sys_bd.png)

### Software Design
TODO