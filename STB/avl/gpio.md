# gpio使用

GPIO函数介绍
- GPIO_F_SET(a,d)//GPIO口使能设置，d=1 enable. D=0 disable
- GPIO_M_SET(a,d)// 设置IO口支持的指令d=0 IOP/d=1 RISC
- GPIO_E_SET(a,d)//设置IO口输入d=0/输出模式d=1
如果没有上面的设置，直接使用下面函数是不对的。
- GPIO_O_SET(a,d)//设置IO口状态
- GPIO_I_GET(a) //读取IO口状态
