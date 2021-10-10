# commonly_used_ip_core
学习和开发过程中经常使用的ip core(只包含HDL代码)
## xdma_irq_ctrl
xdma的中断控制ip core，使用标准的AXI4-Lite协议，实现xdma的中断产生和清除。<br>
### (1)Block Diagram
![bd](https://github.com/XiaogegeChen/commonly_used_ip_core/blob/main/xdma_irq_ctrl/pic/bd.png)

```S_AXI```端口用于控制中断的写入和清除，```xdma_user_irq_req```端口用于提供中断信号给xdma，连接再xdma的```usr_irq_req```端口。

### (2)Parameters
![params](https://github.com/XiaogegeChen/commonly_used_ip_core/blob/main/xdma_irq_ctrl/pic/cfg.png)

```Intr Count```用于配置中断数量，支持的范围为```[1...16]```

### (3)System Design Exampel
![sys_bd](https://github.com/XiaogegeChen/commonly_used_ip_core/blob/main/xdma_irq_ctrl/pic/sys_bd.png)

在系统设计中，中断可以由Host产生，也可以由FPGA内部产生。产生中断时只需通过```xdma_irq_ctrl/S_AXI```端口向对应的寄存器写入```0x1```，当Host接收到中断后再通过```xdma_irq_ctrl/S_AXI```端口向对应的寄存器写入```0x0```以清除中断。
### (4)Software Design
``` C
int axi_lite_fd;
int intr0_fd;
int intr0_offset;
void* map_base;

void my_func() {
    while(1) {
        int tmp;
        read(intr0_fd, &tmp, 4);
        // TODO: Handle interrupt here

        // clear interrupt
        *(((volatile uint32_t*)(axi_lite_fd + intr0_offset)) + 0) = 0;
    }
}

int main(int argc, char const *argv[]) {
    axi_lite_fd = ...;  // axi lite fd, e.g. "/dev/xdma0_user"
    intr0_fd = ...;  // interrupt fd, e.g. "/dev/xdma0_events_0"
    map_base = ...;  //  mmap()

    int ids[4] = {0, 1, 2, 3};
    if(pthread_create(ids+1, NULL, (void*) my_func, NULL)){
        return -1;
    }

    return 0;
}
```

## xdma_irq_ctrl