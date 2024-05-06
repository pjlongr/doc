# Unicable命令设置


## 描述

它能够将频点搬运到950M-2150M的任何一个位置上，可以实现将高低本振，水平垂直的任意组合的信号在同一根同轴线上传播，这就意味着一根同轴线上可以同时传输多个频点信息而不会互相干扰。

##  SaTCR number

DVBS/S2接收机（电视或机顶盒等）允许输入的频率范围是950M~2150MHz。在这1G左右的频率范围内，最多被平分为8等份，即Unicable设备把950~2150MHz分成几个互不干扰的频段，每个频段叫UB（User Band）；一个Unicable设备最多分配8个UB。

950M—–>1284<->1400<->1516<->1632<->1748<->1864<->1980<->2096<—–2150M

按照惯例，SATCR1 被分配最低 IF 频率，SaTCR2 被分配第二个等等

| SaTCR  | number | IF frequency |
| :----: | :----: | :----------: |
| SaTCR1 |   0    |     1284     |
| SaTCR2 |   1    |     1400     |
| SaTCR3 |   2    |     1516     |
| SaTCR4 |   3    |     1632     |
| SaTCR5 |   4    |     1748     |
| SaTCR6 |   5    |     1864     |
| SaTCR7 |   6    |     1980     |
| SaTCR8 |   7    |     2096     |

##  LNB number

Uincable specification规定最多支持两颗卫星，即8个LNB：
单本振时，是Low band

| Satellite  |                 LNB                 | number |
| :--------: | :---------------------------------: | :----: |
| Position A |  Low band / vertical polarization   |   0    |
|            |  High band / vertical polarization  |   1    |
|            |  Low band /horizontal polarization  |   2    |
|            | High band / horizontal polarization |   3    |
| Position B |  Low band / vertical polarization   |   4    |
|            |  High band / vertical polarization  |   5    |
|            |  Low band /horizontal polarization  |   6    |
|            | High band / horizontal polarization |   7    |

##   SaTCR DiSEqC frames

DiSEqC frame requires 5 bytes:

| Framing | Address | Command | Data1 | Data2 |
| ------- | ------- | ------- | ----- | ----- |
| E0      | 10/11   | 5A/5B   | XX    | XX    |

FRAMING :
		0xE0 (command from master, no reply, first transmission)

ADDRESS:
		0x00, 0x10(Any LNB, Switcher or SMATV),
		0x11(LNB)

COMMAND:
		0x5A (Normal operation),
		0x5B (Special operation)

##  ODU_ChannelChange

改变当前所选的频道

```
E0 10 5A channel_byte1 channel_byte2
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/1284c2d57c7d4352b6367bbdd846ba63.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d92ef1e42eb441f885702a3ecb0eca7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA6b6Z5LiA5LiA,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/bdc5567f4a1447fbb0cb5fe25194111a.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/c3f7eef4c9274891addaf0f3a1ef0cf1.png#pic_center)
**Ftransponder：** 当前频点的频率
**FLO：** 当前选择的本振，双本振时，如果频率小于11700 MHz，选择低的那个
**F bpf**：SaTCR number对应的IF frequency
**例如：**
频点：10800/3000 H

本振：9750/10600

卫星：SatA

SaTCR number： 1

SaTCR IF frequency：1420 MHz

T=((10800-9750）+1420)/4-350=267=0x10B=000100001011

channel_byte1= 001 010 01=0010 1001=29

channel_byte2=00001011=0B

**命令：**
```
E0 10 5A 29 0B
```
##  ODU_PowerOFF

将对应TUNER关闭，释放所使用的UB通道

```
E0 10 5A poweroff_byte 00
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/8911ebfcd03c481795c5697bf09f9a94.png#pic_center)
**例如：**

频点：10800/3000 H

本振：9750/10600

卫星：SatA

SaTCR number： 1

SaTCR IF frequency：1420 MHz

poweroff_byte=001 00000=0010 0000=20

**命令：**

```
E0 10 5A 20 00
```
相关链接
[AN2056](http://www.bdtic.com/DownLoad/ST/AN2056.pdf):      Extention of the SRC DiSEqC 1 standard for control of Satellite Channel Router based one-cable LNB
[Unicable 1.0技术摘要](https://blog.csdn.net/qq_38296382/article/details/88690453)
[DVBS/S2在数字电视系统中的应用 五 （Single Cable介绍）](https://www.it610.com/article/5068038.htm)