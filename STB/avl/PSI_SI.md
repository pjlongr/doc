<a name="WSMCv"></a>
## 学习网站
[http://www.onelib.biz/doc/stb/index.html](http://www.onelib.biz/doc/stb/index.html)
<a name="jO53T"></a>
## PSI /SI 表的映射简图
从PAT表开始，我们通过program_map_PID找到了对应的PMT表。<br />在PMT表中，我们通过program_number与SDT表中的service_id关联起来。<br />在EIT表中，我们通过service_id与SDT表中的service_id以及PMT表的program_number关联起来。<br />![30.png](https://cdn.nlark.com/yuque/0/2022/png/29233958/1658394530513-1800e2e2-7d21-4eaa-af53-1d55142513dd.png#clientId=uccd39fca-8cea-4&from=ui&id=u91db5df5&originHeight=748&originWidth=1292&originalType=binary&ratio=1&rotation=0&showTitle=false&size=183717&status=done&style=none&taskId=u86311b6a-4b11-48d9-b0c1-de462c1a86b&title=)
<a name="tPtdl"></a>
## TS流的格式
<a name="dX9Qn"></a>
### TS流的包结构
TS流是基于Packet的位流格式，即由n个包组成；每个包是188个字节（或204个字节，在188个字节后加上了16字节的CRC校验数据，其他格式一样）。 下图是一个TS流，以第k个包(Package)为例：<br />![](https://cdn.nlark.com/yuque/0/2022/jpeg/29233958/1658451407553-f80b2db1-41a5-4493-ae06-ca9f5bb327f5.jpeg#clientId=u09c89c08-e3e7-4&from=paste&id=u9a353ddf&originHeight=131&originWidth=653&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ue918d44f-a9f4-4a37-b9ee-e0c291d7fe2&title=)

| **Packet Header（包头）信息说明 (4bytes )** |  |  |  |
| --- | --- | --- | --- |
| # | 标识 | 位数 | 说明 |
| 0 | sync_byte | 8bits | 同步字节，固定是0x47 |
| 1 | transport_error_indicator | 1bits | 错误指示信息（1：该包至少有1bits传输错误） |
| 2 | payload_unit_start_indicator | 1bits | 负载单元开始标志（packet不满188字节时需填充） |
| 3 | transport_priority | 1bits | 传输优先级标志（1：优先级高） |
| 4 | **PID** | 13bits | **Packet ID号码，唯一的号码对应不同的包** |
| 5 | transport_scrambling_control | 2bits | 加密标志（00：未加密；其他表示已加密） |
| 6 | adaptation_field_control | 2bits | 附加区域控制 |
| 7 | continuity_counter | 4bits | 包递增计数器 |

如果一个TS流中的一个Packet的Packet Header中的PID是0x0000，那么这个Packet的Packet Data就是DVB的PAT表而非其他类型数据（如Video、Audio或其他业务信息）。

| TS流中PID值（关于SI部分，这些值是固定的，不可更改） |  |  |
| --- | --- | --- |
| 表 | PID值 | 说明 |
| PAT | 0x0000 | 关联节目编号和节目映射表 PID |
| CAT  | 0x0001 |  |
| TSDT | 0x0002 |  |
| 预留 | 0x0003~0x000F |  |
| NIT,ST | 0x0010 |  |
| SDT、BAT、ST | 0x0011 |  |
| EIT、ST、CIT(TS 102 323 [13]) | 0x0012 |  |
| RST、RT | 0x0013 |  |
| TDT、TOT、ST | 0x0014 |  |
| 网络同步 | 0x0015 |  |
| RNT (TS 102 323[13]) | 0x0016 |  |
| 预留未来使用 | 0x0017 to 0x001B |  |
| 带内信令 | 0x001C |  |
| 测量 | 0x001D |  |
| DIT | 0x001E |  |
| SIT | 0x001F |  |

<a name="DnIqQ"></a>
### TS流的其中一个Packet中的Packet Header实例分析
Packet 数据
```
47 02 00 1B 0E 7A 43 3E 0C 19 B3 00 88 7D 12 71 
11 4D 61 9C 02 CC DB 9E 70 C4 D1 4B 2F A5 20 02 
C0 A5 5D 68 18 F4 73 9A ED 86 F3 85 8F 8E C7 77 
17 EE A3 0B 5E 36 49 D2 E4 DD 72 3F F4 4C FE F2 
BD EB 72 90 A1 52 1A 21 08 03 7D E0 C2 8A EB 22 
2A 4E 26 06 94 5A C0 B6 48 FE 2F C9 29 00 36 C5 
E3 84 28 83 57 4E CD 20 94 8D 92 83 C8 E6 1E A7 
83 59 00 50 B0 1B 20 73 3B 9D BE 16 FA 26 14 CE 
21 C5 AD E2 57 01 B0 19 03 08 1E 95 9F FA A3 28 
FE 67 72 88 17 C4 4B 20 FE 46 23 6E D1 60 84 00 
E4 52 0D 2D 61 68 75 88 D5 00 E7 EB 39 E7 2C 8B 
12 BF 31 0E 1C 73 20 81 30 02 99 6E 
```
Packet header 是4bytes（32bits）：47 02 00 1B ，转换成二进制：01000111 00000010 00000000 00011011<br />根据Packet header 结构，将二进制的值分成8部分<br />01000111 0 0 0 0001000000000 00 01 1011

| **Packet Header（包头）信息说明 (4bytes )** |  |  |  |  |
| --- | --- | --- | --- | --- |
| # | 标识 | 位数 | 值 | 说明 |
| 0 | sync_byte | 8bits | 01000111 | 固定是0x47 |
| 1 | transport_error_indicator | 1bits | 0 | 值为0，表示当前包没有发生传输错误。错误指示信息（1：该包至少有1bits传输错误） |
| 2 | payload_unit_start_indicator | 1bits | 0 | 值为0，含义参考ISO13818-1标准文档。负载单元开始标志（packet不满188字节时需填充） |
| 3 | transport_priority | 1bits | 0 | 值为0，表示当前包是低优先级。传输优先级标志（1：优先级高） |
| 4 | **PID** | 13bits | 0001000000000 | PID=<br />0001000000000即0x200,是Video PID。Packet ID号码，唯一的号码对应不同的包 |
| 5 | transport_scrambling_control | 2bits | 00 | 值为0x00，表示节目没有加密。加密标志（00：未加密；其他表示已加密） |
| 6 | adaptation_field_control | 2bits | 01 | 值为0x01,具体含义请参考ISO13818-1。附加区域控制 |
| 7 | continuity_counter | 4bits | 1011 | 值为0x0B,表示当前传送的相同类型的包是第13个。包递增计数器 |

<a name="wCDUz"></a>
# PSI 
PSI是对单一TS流的描述，是TS流中的引导信息。<br />PSI信息由节目关联表PAT、条件接收表CAT、节目映射表PMT和网络信息表NIT组成，这些表会被插入到TS流中。 PSI信息是对单一TS流的描述，它是TS流的引导信息；PSI信息指定了如何从一个携带多个节目的传输流中找到指定的节目。 下面给出的是节目引导信息（或称节目特定信息，PSI）的四个表结构。

| **结构名** | **中文** | **所定义标准** | **PID** | **描述** |
| --- | --- | --- | --- | --- |
| PAT | 节目关联表 | MPEG2标准 | 0x0000 | 将节目号码和节目映射表PID相关联，是获取数据的开始 |
| PMT | 节目映射表 | MPEG2标准 | PAT中标识 | 指定一个或多个节目的PID |
| CAT | 条件接收表 | MPEG2标准 | 0x0001 | 将一个或多个专用EMM流分别与唯一的PID相关联 |
| NIT | 网络信息表 | SI标准 | PAT中标识 | 描述整个网络，如多少个TS流、频点和调制方式等信息 |

<a name="iyN8v"></a>
## PAT表
<a name="bilXV"></a>
### PAT表概述：
**PAT表(Program Association Table，节目关联表)** 定义了当前TS流中所有的节目，其PID为0x0000，它是PSI的根节点，要查寻找节目必须从PAT表开始查找。<br />节目关联表PAT的意义在于，它描述了当前TS流中包含了哪些PID； 只有根据获得的PID，用户才可以以此作为凭据找出其他表（如PMT表）及其信息。 所以 **PAT是机顶盒接收的入口点，是它获取数据的开始**； 要保证一个TS流能被正常接收，则至少要有一个完整有效的PAT。<br />PAT表携带以下信息:

<a name="v7v5G"></a>
### PAT 节目关联表结构
![11.png](https://cdn.nlark.com/yuque/0/2022/png/29233958/1658458487081-90c56754-fcc2-4636-ac2b-b3f59849c64b.png#clientId=u09c89c08-e3e7-4&from=ui&id=uad2f5a87&originHeight=484&originWidth=827&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67560&status=done&style=none&taskId=u715de06c-2969-43f2-b1bd-ad37cce3a2b&title=)<br />主要字段：

   - **table_id**：PAT的table_id应为0x00
   - **transport_stream_id（传输流标志）**：用以标识来源于网络中任何其他复合流的TS流
   - **program_number（节目号）**：规定program_map_PID可适用的节目。当值为0x0000时，其后的PID参照将是网络PID。它可以作为一个指示符号，例如用于广播通道。该号码标志TS流中的一个频道，该频道可以包含很多的节目(即可以包含多个Video PID和Audio PID)
   - **network_PID（网络PID）**：仅当program_number为0x00时使用
   - **program_map_PID（节目映射PID）**：据此找出相应的PMT表，表示本频道使用哪个PID做为PMT的PID。因为可以有很多的业务，因此DVB规定PMT的PID可以由用户自己定义。
<a name="zzfMn"></a>
### Table_id 分配
table_id 字段标识传输流 PSI 部分的内容

| 值 | 描述 |
| --- | --- |
| 0x00 | program_association_section(节目关联部分) |
| 0x01 | conditional_access_section (CA_section)（CA部分） |
| 0x02 | TS_program_map_section |
| 0x03 | TS_description_section |
| 0x04 | ISO_IEC_14496_scene_description_section |
| 0x05 | ISO_IEC_14496_object_descriptor_section |
| 0x06~0x37 | ITU-T Rec. H.222.0 &#124; ISO/IEC 13818-1 reserved  |
| 0x38~0x3F | Defined in ISO/IEC 13818-6  |
| 0x40~0x4F | User private  |
| 0xFF | forbidden |

<a name="qjdTG"></a>
### PAT packet分析
<a name="dRCOF"></a>
#### pat packet数据
```
47 40 00 15 00 00 B0 85 00 07 CB 00 00 02 BC E1 
0A 02 BD E1 0D 02 BF E1 0F 02 C0 E1 09 02 C1 E1 
04 02 C2 E1 02 02 C4 E1 01 02 C5 E1 0E 02 C8 E1 
03 02 C9 E1 0B 02 CB E1 06 02 D1 E1 10 02 D4 E1 
13 02 D5 E1 14 02 D6 E1 15 02 D7 E1 16 02 D8 E1 
17 02 DA E1 19 02 DB E3 20 02 DE E3 23 02 DF E3 
24 02 E0 E3 25 02 E1 E3 26 02 E2 E3 27 02 E3 E3 
28 02 E4 E4 01 02 E5 E3 29 02 E6 E3 30 02 FE E1 
6B 02 FF E1 05 03 16 E1 50 AC 61 F3 B3 FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF 
```
| **包头(Package Header)(4 bytes)** | **包数据(Package Data)(184 bytes)** |
| --- | --- |
| 47 40 00 15 | 00 00 B0 85 00 07 CB 00 00 02 BC E1 .....FF FF FF |

根据 packet header的结构可以的到：

- PID：0x0000 （PAT表）
- payload_unit_start_indicator: 1

如果payload_unit_start_indicator=1，在包头后需要除去一个字节才是有效数据.<br />原始数据：<br />00 00 B0 85 00 07 CB 00 00 02 BC E1<br />有效数据：<br />      00 B0 85 00 07 CB 00 00 02 BC E1
<a name="ogdXD"></a>
#### PAT packet数据分析：
| # | 字段名 | 占位 | 值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| 0 | table_id | 8 bits | 00000000 | 第0个字节：00 | PAT的table_id只能是0x00 |
| 1 | section_syntax_indicator | 1 bits | 01(0x01) | 第1~2字节：<br />B0 85<br />二进制：10110000 10000101<br /> | 段语法标志位，固定为1 |
| 2 | zero | 1 bits | 00(0x00) |  | <br /> |
| 3 | reserved | 2 bits | 11（0x03） |  | <br /> |
| 4 | section_length | 12 bits | 0000 10000101（0x85,） |  | 意思是 段长度为133（0x85）字节。从<br />section_length后面的字节开始算，不得超过 1021 字节(0x3FD) |
| 5 | transport_stream_id | 16 bits | 0x07 | 第3~4个字节：<br />00 07<br />二进制：<br />0000 0000<br />0000 1111 | TS的识别号 |
| 6 | reserved | 2 bits | 11 (0x03) | 第5个字节：<br />CB<br />二进制：<br />1100 1011 | <br /> |
| 7 | version_number | 5 bits | 00 101(0x05) |  | 一旦PAT有变化，版本号加1 |
| 8 | current_next_indicator | 1 bits  | 1 (0x01) |  | 当前传送的PAT表可以使用，若为0则要等待下一个表 |
| 9 | section_number | 8 bits | 00000000（0x00） | 第6个字节：<br />00 | 给出section号，在sub_table中，<br />第一个section其section_number为"0x00",<br />每增加一个section,section_number加一 |
| 10 | last_section_number | 8 bits | 0000 0000<br />(0x00) | 第7个字节：<br />00 | sub_table中最后一个section的section_number |
| **循环开始** |  |  |  |  |  |
| <br /> | program_number | 16 bits | <br /> | 4 bytes | section_length的值是133，减去5个bytes（5~10），减去4个bytes(CRC)。<br />PMT数据是：133-5-4=124 bytes<br />算出节目数：124/4=31 个节目 |
| <br /> | reserved | 3 bits | <br /> |  |  |
| <br /> | network_id<br />或<br />program_map_PID | 13 bits | <br /> |  |  |
| **循环结束** |  |  |  |  |  |
| <br /> | CRC_32 | 32 bits | <br /> | 最后4个bytes | <br /> |
| 如果没满188个字节，后面的字节填充 0xFF |  |  |  |  |  |
| 

<a name="QfNrN"></a>
### 解析循环部分
47 40 00 15 00 00 B0 85 00 07 CB 00 00 02 BC E1 <br />0A 02 BD E1 0D 02 BF E1 0F 02 C0 E1 09 02 C1 E1 <br />04 02 C2 E1 02 02 C4 E1 01 02 C5 E1 0E 02 C8 E1 <br />03 02 C9 E1 0B 02 CB E1 06 02 D1 E1 10 02 D4 E1 <br />13 02 D5 E1 14 02 D6 E1 15 02 D7 E1 16 02 D8 E1 <br />17 02 DA E1 19 02 DB E3 20 02 DE E3 23 02 DF E3 <br />24 02 E0 E3 25 02 E1 E3 26 02 E2 E3 27 02 E3 E3 <br />28 02 E4 E4 01 02 E5 E3 29 02 E6 E3 30 02 FE E1 <br />6B 02 FF E1 05 03 16 E1 50 AC 61 F3 B3<br />根据上面的分析：<br />Packet header: 47 40 00 15<br />payload_unit_start_indicator=1 跳过一个字节：00<br />包的信息8个字节：00 B0 85 00 07 CB 00 00<br />节目信息开始的位置：02 BC E1......F3 B3<br />根据上面的表格分析，每次循环都是4个字节（32 bits）,根据section_length的值算出一共要循环31次。

<a name="vzILF"></a>
#### 第一个循环：
数据：02 BC E1 0A （0000 0010 1011 1100 1110 0001 0000 1010）

| # | 字段名 | 占位 | 值 | 说明 |
| --- | --- | --- | --- | --- |
| 1 | program_number | 16 bits | 0000 0010 1011 1100<br />(0x02BC) | program_number=0x2BC=700 |
| 2 | reserved | 3 bits | 111(0x07) | <br /> |
| 3 | program_map_PID或<br />network_id | 13 bits | 0 0001 0000 1010(0x010a) | program_map_PID=0x010a=266,只有program_number=0，这里才是<br />network_id |

可以得出：program_number=700  program_map_PID=266<br />通过这个循环，我们可以知道，在这个TS中，有一个节目号为0x2BC(即十进制700)的节目，其PMT的PID为0x010a。那么要想获取这个节目的详细信息，就要去解析PID为0x010a的PMT表。（关于PMT表的解析可参看下一节内容）<br />**PAT就是一个总入口**。PAT告诉了我们，这个TS流中有几个节目，以及它们的PMT PID分别是多少。有了PMT的PID，我们就可以继续下一步了。
<a name="kerfb"></a>
## PMT表
<a name="YRI5s"></a>
### PMT表概述
节目映射表PMT的意义在于，它给出了节目号与组成这个节目元素之间的映射； 也就是说，PMT是连接节目号与节目元素的桥梁。 我们知道，一个电视节目至少包含了视频和音频数据，而每一个节目的视音频数据都是以包的形式在TS流中传输的； 所以说，一个TS流包含了多个节目的视频和音频数据包。 要想过滤出一个TS流中其中一个节目的视频和音频，则需要知道这个节目中视频和音频的标识号PID。 PMT表的作用就在于，它提供了每个节目视频、音频（或其他）数据包的PID。

- PMT表主要提供节目numbers和节目elements之间的映射关系。
- PID = 值由编码器选择
- table_id = 0x02
<a name="nIKRV"></a>
### stream_type 分配表
| **stream_type** | **描述** |
| --- | --- |
| 0x00 | ITU-T &#124; ISO/IEC Reserved，国际标准保留 |
| 0x01 | ISO/IEC 11172 Video，视频 |
| 0x02 | ITU-T Rec. H.262 &#124; ISO/IEC 13818-2 Video or ISO/IEC 11172-2 constrained parameter video stream，视频或受限参数视频流 |
| 0x03 | ISO/IEC 11172 Audio，音频 |
| 0x04 | ISO/IEC 13818-3 Audio，音频 |
| 0x05 | ITU-T Rec. H.222.0 &#124; ISO/IEC 13818-1 private_sections |
| 0x06 | ITU-T Rec. H.222.0 &#124; ISO/IEC 13818-1 PES packets containing private data,包含专用数据的PES分组 |
| 0x07 | ISO/IEC 13522 MHEG |
| 0x08 | ITU-T Rec. H.222.0 &#124; ISO/IEC 13818-1 Annex A DSM CC |
| 0x09 | ITU-T Rec.H.222.1 |
| 0x0A | ISO/IEC 13818-6 type A |
| 0x0B | ISO/IEC 13818-6 type B |
| 0x0C | ISO/IEC 13818-6 type C |
| 0x0D | ISO/IEC 13818-6 type D |
| 0x0E | ISO/IEC 13818-1 auxiliary |
| 0x0F~0x7F | ITU-T Rec. H.222.0 &#124; ISO/IEC 13818-1 Reserved，GB/T保留 |
| 0x80~0xFF | User Private，用户专用 |

<a name="MuSnB"></a>
###  Program and program element descriptors  （节目和节目元素描述符）
表位于 ISO 13818-1[1].pdf

|  descriptor_tag   |  TS   |  PS   |  Identification   |
| --- | --- | --- | --- |
|  0   |  n/a   |  n/a   |  Reserved   |
|  1   |  n/a   |  n/a   |  Reserved   |
|  2   |  X   |  X   |  video_stream_descriptor   |
|  3   |  X   |  X   |  audio_stream_descriptor   |
|  4   |  X   |  X   |  hierarchy_descriptor   |
|  5   |  X   |  X   |  registration_descriptor   |
|  6   |  X   |  X   |  data_stream_alignment_descriptor   |
| 7 | X   | X   |  target_background_grid_descriptor   |
| 8 | X   | X   |  Video_window_descriptor   |
| 9 | X   | X   |  CA_descriptor   |
| 10 | X   | X   |  ISO_639_language_descriptor   |
| 11 | X   | X   |  System_clock_descriptor   |
| 12 | X   | X   |  Multiplex_buffer_utilization_descriptor   |
| 13 | X   | X   |  Copyright_descriptor   |
| 14 | X   | <br /> |  Maximum_bitrate_descriptor   |
| 15 | X   | X   |  Private_data_indicator_descriptor   |
| 16 | X   | X   |  Smoothing_buffer_descriptor   |
| 17 | X   | <br /> |  STD_descriptor   |
| 18 | X   | X   |  IBP_descriptor   |
| 19~26 | X   | <br /> |  Defined in ISO/IEC 13818-6   |
| 27 | X   | X   |  MPEG-4_video_descriptor   |
| 28 | X   | X   |  MPEG-4_audio_descriptor   |
| 29 | X   | X   |  IOD_descriptor   |
| 30 | X   | <br /> |  SL_descriptor   |
| 31 | X   | X   |  FMC_descriptor   |
| 32 | X   | X   |  External_ES_ID_descriptor   |
| 33 | X   | X   |  MuxCode_descriptor   |
| 34 | X   | X   |  FmxBufferSize_descriptor |
| 35 | X   | <br /> |  MultiplexBuffer_descriptor   |
| 36~63 |  n/a   |  n/a   |  ITU-T Rec. H.222.0 &#124; ISO/IEC 13818-1 Reserved   |
| 64~255 |  n/a   |  n/a   |  User Private   |

<a name="O4OzW"></a>
### PMT表结构
![22.png](https://cdn.nlark.com/yuque/0/2022/png/29233958/1658469214739-3bd961d8-a959-4b15-b6d2-955c7c77c464.png#clientId=ue514ad4f-6212-4&from=ui&id=u2e53afbb&originHeight=620&originWidth=852&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85690&status=done&style=none&taskId=u4f597bda-c39b-4a44-bd13-81a7610d366&title=)
<a name="VLYG9"></a>
### PMT包分析
PMT packet数据
<a name="cQgX0"></a>
#### 找到PID为0x010a的packet
```
47 41 0A 19 00 02 B0 3E 02 BC C3 00 00 E2 08 F0 
00 02 E2 08 F0 00 03 E2 DA F0 06 0A 04 61 72 61 
00 03 E2 DB F0 06 0A 04 65 6E 67 00 03 E2 DC F0 
06 0A 04 66 69 6E 00 03 E0 88 F0 06 0A 04 73 70 
61 00 30 21 04 1E FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF 
```
| **包头(Package Header)(4 bytes)** | **包数据(Package Data)(184 bytes)** |
| --- | --- |
| 47 41 0A 19 | 00 02 B0 3E 02 BC C3 00 00 E2 08 F0 .....FF FF FF |

根据 packet header的结构可以的到：

- PMT PID：0x010A
- payload_unit_start_indicator: 1

如果payload_unit_start_indicator=1，在包头后需要除去一个字节才是有效数据.<br />原始数据：<br />00 02 B0 3E 02 BC C3 00 00 E2 08 F0<br />有效数据：<br />      02 B0 3E 02 BC C3 00 00 E2 08 F0 00 02 E2 08 F0 00 03 E2 DA F0 06 0A 04 61 72 61
<a name="gW0bJ"></a>
##### PMT packet 数据分析
| # | 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| 0 | table_id | 8 | 0x02 | 第0字节：<br />02 | 表标识符 |
| 1 | Section_syntax_indicator | 1 | 1（0x01） | 第1~2字节：<br />B0 3E<br />二进制：<br />1011 0000 0011 1110 | 段语法指示符，通常设置为“1” |
| 2 | "0" | 1 | 0 (0x00) |  | zero |
| 3 | Reserved | 2 | 11 (0x03) |  | 保留 |
| 4 | Section_length | 12 | 0000 0011 1110 (0x3E) |  | 段长度，12位，指出了此Section的长度，头两位为“00”，值不超过1021。从section_length 字段后面开始并包括 CRC 的部分的字节数 |
| 5 | program_number | 16 | 0000 0010 1011 1100<br />（0x02BC） | 第3~4字节：<br />02 BC<br />二进制：<br />0000 0010 1011 1100 | 节目号，与service_id对应 |
| 6 | Reserved | 2 | 11 (0x03) | 第5字节：<br />C3<br />二进制：<br />11000011 | 保留 |
| 7 | Version_number | 5 | 00001(0x01) |  | sub_table的版本号，值为0~31，当sub_table信息改变时，值加1，若值已为31，则变化后值为0（循环），若curren_next_indicator的值为“1”，version_number指当前sub_table,若curren_next_indicator的值为“0”，version_number指下一个sub_table |
| 8 | Current_next_indicator | 1 | 1(0x01) |  | 当前后续指示符，1位，“1”指sub_table是current, “0”指sub_table是next |
| 9 | Section_number | 8 | 0x00 | 第6字节：<br />00  | 段号，8位,给出section号，在sub_table中，第一个section其section_number为"0x00"，每增加一个section，section_number加一 |
| 10 | last_section_number | 8 | 0x00 | 第7字节：<br />00 | 最后段号，sub_table中最后一个section的section_number |
| 11 | reserved | 3 | 111 （0x07） | 第8~9字节：<br />E2 08<br />二进制：<br />1110 0010<br />0000 1000 | 保留 |
| 12 | PCR_PID | 13 | 0 0010<br />0000 1000<br />(0x0208) |  | 13位, 指定了包含NIT的TS包的PID |
| 13 | reserved | 4 | 1111 （0x0F） | 第10~11字节：<br />F0 00<br />二进制：<br />1111 0000<br />0000 0000 | 保留 |
| 14 | program_info_length | 12 | 0000<br />0000 0000<br />(0x00) |  | 头两位为“00”<br />节目信息长度<br />(之后的是N个描述符结构,一般可以忽略掉,这个字段就代表描述符总的长度,单位是Bytes)<br />紧接着就是频道内部包含的节目类型和对应的PID号码了 |
| <br /> | 循环开始 |  |  |  | program_info_length==0;这里忽略掉 |
| <br /> | **descriptor（）** |  |  |  |  |
| <br /> | 循环结束 |  |  |  |  |
| <br /> | 循环开始 |  |  |  |  |
| <br /> | stream_type | 8 | <br /> | <br /> | 8位,指定了节目element的类型 |
| <br /> | reserved | 3 | <br /> | <br /> | 保留 |
| <br /> | elementary_PID | 13 | <br /> | <br /> | 13位,指定了包含program element的TS包的PID |
| <br /> | reserved | 4 | <br /> | <br /> | 保留 |
| <br /> | ES_info_length | 12 | <br /> | <br /> | 头两位为"00" |
| <br /> | 循环开始 |  |  |  | <br /> |
| <br /> | **descriptor（）** |  |  |  | <br /> |
| <br /> | 循环结束 |  |  |  | <br /> |
| <br /> | 循环结束 |  |  |  |  |
| <br /> | CRC | 32 | <br /> | <br /> | 32位, 表示CRC的值 |

开始循环解频道内部包含的节目类型和对应的PID号码：<br />从上面解析后，可以得到节目数据：<br />03 E2 DA F0 06 0A 04 61 72 61 <br />00 03 E2 DB F0 06 0A 04 65 6E 67 00 03 E2 DC F0 <br />06 0A 04 66 69 6E 00 03 E0 88 F0 06 0A 04 73 70 <br />61 00 30 21 04 1E
<a name="W7uCq"></a>
##### 第一个单元流：
| <br /> | 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| <br /> | 循环开始 |  |  |  |  |
| <br /> | stream_type | 8 | 0x03 | 第0字节：<br />03 | 8位,指定了节目element的类型。<br />0x03经查<br />stream_type表，得知这是音频信息 |
| <br /> | reserved | 3 | 111（0x07） | 第1~2字节：<br />E2 DA<br />二进制：<br />1110 0010<br />1101 1010 | 保留 |
| <br /> | elementary_PID | 13 | 0 0010<br />1101 1010<br />(0x02DA) |  | 13位,指定了包含program element的TS包的PID。<br />由于从stream_type得知这是描述音频信息，所以这个是音频PID |
| <br /> | reserved | 4 | 1111 （0x000F） | 第3~4字节：<br />F0 06<br />二进制：<br />1111 0000 <br />0000 0110 | 保留 |
| <br /> | ES_info_length | 12 | 0000 <br />0000 0110<br />(0x0006) |  | 头两位为"00"<br /><br />6 bytes |
| <br /> | 循环开始 |  |  |  | 通过descriptor_tag查Program and program element descriptors 表（ISO13818-），决定使用哪个descriptors 结构。descriptor_tag是<br />ES_info_length后的第一个字节。这里descriptor_tag=0A,查表可知使用ISO_639_language_descriptor |
| <br /> | **descriptor（）** | <br /> | <br /> | 0A 04 61 72 61 <br />00 |  |
| <br /> | 循环结束 |  |  |  |  |
| <br /> | 循环结束 |  |  |  |  |

0A 04 61 72 61 00<br />通过ISO_639_language_descriptor将ES_Info解析出来：

| ISO_639_language_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x0A | <br /> | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x0A标识<br />ISO_639_language_descriptor |
| descriptor_length | 8 | 0x04 | <br /> | 描述符长度是一个 8 位字段，指定描述符的立即字节数<br />跟随描述符长度字段。这里是4个字节 |
| 循环开始 |  |  |  | 由descriptor_length是4个字节，一次循环就要4个字节。所以只循环一次 |
| ISO_639_language_code | 24 | 0x617261 | <br /> | 查unicode表可以知道是字符：ara |
| audio_type | 8 | 0x00 | <br /> | <br /> |
| 循环结束 |  |  |  |  |

<a name="WI6DO"></a>
#### 找到PID为0x010f的packet
```
47 41 0F 1A 00 02 B0 2D 02 BF C5 00 00 E0 81 F0 
10 0B 02 4A 1F 09 04 17 08 FE 17 09 04 06 04 E1 
30 02 E2 01 F0 00 04 E2 94 F0 06 0A 04 61 72 61 
01 16 E3 D0 3E FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF 
```
| **包头(Package Header)(4 bytes)** | **包数据(Package Data)(184 bytes)** |
| --- | --- |
| 47 41 0F 1A | 00 02 B0 2D 02 BF C5 00 00 E0 81 F0 .....FF FF FF |

根据 packet header的结构可以的到：

- PMT PID：0x010F
- payload_unit_start_indicator: 1

如果payload_unit_start_indicator=1，在包头后需要除去一个字节才是有效数据.<br />原始数据：<br />00 02 B0 2D 02 BF C5 00 00 E0 81 F0<br />有效数据：<br />     02 B0 2D 02 BF C5 00 00 E0 81 F0 10 0B 02 4A 1F 09 04 17 08 FE 17 09 04 06 04 E1 
<a name="zG8eM"></a>
##### PMT packet 数据分析
| # | 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| 0 | table_id | 8 | 0x02 | 第0字节：<br />02 | 表标识符 |
| 1 | Section_syntax_indicator | 1 | 1（0x01） | 第1~2字节：<br />B0 2D<br />二进制：<br />1011 0000 0010 1101 | 段语法指示符，通常设置为“1” |
| 2 | "0" | 1 | 0 (0x00) |  | zero |
| 3 | Reserved | 2 | 11 (0x03) |  | 保留 |
| 4 | Section_length | 12 | 0000 0010 1101(0x2D) |  | 段长度，12位，指出了此Section的长度，头两位为“00”，值不超过1021。从section_length 字段后面开始并包括 CRC 的部分的字节数 |
| 5 | program_number | 16 | <br /><br />（0x02BF） | 第3~4字节：<br />02 BF<br />二进制：<br />0000 0010<br />1011 1111 | 节目号，与service_id对应 |
| 6 | Reserved | 2 | 11 (0x03) | 第5字节：<br />C5<br />二进制：<br />1100 0101 | 保留 |
| 7 | Version_number | 5 | 00 010(0x02) |  | sub_table的版本号，值为0~31，当sub_table信息改变时，值加1，若值已为31，则变化后值为0（循环），若curren_next_indicator的值为“1”，version_number指当前sub_table,若curren_next_indicator的值为“0”，version_number指下一个sub_table |
| 8 | Current_next_indicator | 1 | 1(0x01) |  | 当前后续指示符，1位，“1”指sub_table是current, “0”指sub_table是next |
| 9 | Section_number | 8 | 0x00 | 第6字节：<br />00  | 段号，8位,给出section号，在sub_table中，第一个section其section_number为"0x00"，每增加一个section，section_number加一 |
| 10 | last_section_number | 8 | 0x00 | 第7字节：<br />00 | 最后段号，sub_table中最后一个section的section_number |
| 11 | reserved | 3 | 111 （0x07） | 第8~9字节：<br />E0 81<br />二进制：<br />1110 0000 1000 0001 | 保留 |
| 12 | PCR_PID | 13 | 0 0000 1000 0001<br />(0x0081) |  | 13位, 指定了包含NIT的TS包的PID |
| 13 | reserved | 4 | 1111 （0x0F） | 第10~11字节：<br />F0 10<br />二进制：<br />1111 0000<br />0001 0000 | 保留 |
| 14 | program_info_length | 12 | 0000<br />0001 0000<br />(0x10) |  | 头两位为“00”<br />节目信息长度<br />(之后的是N个描述符结构,一般可以忽略掉,这个字段就代表描述符总的长度,单位是Bytes)<br />紧接着就是频道内部包含的节目类型和对应的PID号码了 |
| <br /> | 循环开始 |  |  |  | program_info_length==16;进行循环 |
| <br /> | **descriptor（）** |  |  |  |  |
| <br /> | 循环结束 |  |  |  |  |
| <br /> | 循环开始 |  |  |  |  |
| <br /> | stream_type | 8 | <br /> | <br /> | 8位,指定了节目element的类型 |
| <br /> | reserved | 3 | <br /> | <br /> | 保留 |
| <br /> | elementary_PID | 13 | <br /> | <br /> | 13位,指定了包含program element的TS包的PID |
| <br /> | reserved | 4 | <br /> | <br /> | 保留 |
| <br /> | ES_info_length | 12 | <br /> | <br /> | 头两位为"00" |
| <br /> | 循环开始 |  |  |  | <br /> |
| <br /> | **descriptor（）** |  |  |  | <br /> |
| <br /> | 循环结束 |  |  |  | <br /> |
| <br /> | 循环结束 |  |  |  |  |
| <br /> | CRC | 32 | <br /> | <br /> | 32位, 表示CRC的值 |

<a name="ZIxNN"></a>
##### program_info 描述子进行循环
program_info_length：16，获取16个字节的数据进行循环<br />0B 02 4A 1F  09 04 17 08 FE 17 09 04 06 04 E1 30 <br />通过descriptor_tag查Program and program element descriptors 表（ISO13818-1），决定使用哪个descriptors 结构。<br />descriptor_tag：0B ,查表是 System_clock_descriptor<br />descriptor_length： 02  ，表示descriptor信息内容是 2 bytes

| System_clock_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x0B | 第0字节：<br />0B | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x0标识<br />System_clock_descriptor |
| descriptor_length | 8 | 0x02 | 第1字节：<br />02 | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是2个字节 |
| external_clock_reference_indicator | 1 | 0（0x0） | 第2字节：<br />4A<br />二进制：<br />0100 1010 | 这是一个 1 位指示符。 设置为“1”时，表示系统时钟来自解码器上可能可用的外部频率参考 |
| reserved | 1 | 1(0x01) |  | <br /> |
| clock_accuracy_integer | 6 | 00 1010<br />(0x0A) |  | 与clock_accuracy_exponent 一起，它以百万分之几给出系统时钟的分数频率精度。 |
| clock_accuracy_exponent | 3 | 000 （0x | 第3字节：<br />1F<br />二进制：<br />0001 1111 | 与clock_accuracy_integer 一起，它以百万分之几给出系统时钟的分数频率精度。 |
| reserved | 5 | 1 1111<br />（0x1F） |  | <br /> |

由于16 bytes只解析了4 bytes，继续通过descriptor_tag找对应的descriptor结构<br />0B 02 4A 1F 09 04 17 08 FE 17 09 04 06 04 E1 30 <br />descriptor_tag：09，查Program and program element descriptors 表（ISO13818-1）表可知是CA_descriptor<br />descriptor_length：04， 表示descriptor信息内容是 4 bytes

| CA_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x09 | 第0字节：<br />09 | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x09标识<br />CA_descriptor |
| descriptor_length | 8 | 0x04 | 第1字节：<br />04 | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是4个字节 |
| CA_system_ID | 16 | 0001 0111 0000 1000（0x1708） | 第2~3字节：<br />17 08<br />二进制：<br />0001 0111 0000 1000 | 这是一个 16 位字段，指示适用于相关 ECM 和/或 EMM 流的 CA 系统类型。其编码是私人定义的，ITU-T 未指定 &#124;ISO/IEC |
| reserved | 3 | 111(0x07) | 第4~5字节：<br />FE 17<br />二进制：<br />1111 1110<br />0001 0111 | <br /> |
| CA_PID | 13 | 1 1110<br />0001 0111<br />(0x1E17) |  |  这是一个 13 位字段，指示传输流包的 PID，它应包含 CA 系统的 ECM 或 EMM 信息，如相关 CA_system_ID 所指定。CA_PID 指示的数据包的内容（ECM 或 EMM）是根据 CA_PID 所在的上下文确定的，即TS_program_map_section 或传输流中的 CA 表，或节目流中的 stream_id 字段。 |
| 循环开始 |  |  |  |  |
| private_data_byte | 8 | <br /> | <br /> | descriptor_length只有4字节，前面已经解析完4字节了，没有数据进行循环 |
| 循环结束 |  |  |  |  |

由于16 bytes现在解析了10bytes，还有6bytes，继续通过descriptor_tag找对应的descriptor结构<br />0B 02 4A 1F 09 04 17 08 FE 17 09 04 06 04 E1 30 <br />descriptor_tag：09，查Program and program element descriptors 表（ISO13818-1）表可知是CA_descriptor<br />descriptor_length：04， 表示descriptor信息内容是 4 bytes

| CA_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x09 | 第0字节：<br />09 | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x09标识<br />CA_descriptor |
| descriptor_length | 8 | 0x04 | 第1字节：<br />04 | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是4个字节 |
| CA_system_ID | 16 | 0000 0100 1110 0001<br />（0x0604） | 第2~3字节：<br />06 04<br />二进制：<br />0000 0110<br />0000 0100 | 这是一个 16 位字段，指示适用于相关 ECM 和/或 EMM 流的 CA 系统类型。其编码是私人定义的，ITU-T 未指定 &#124;ISO/IEC |
| reserved | 3 | 111(0x07) | 第4~5字节：<br />E1 30<br />二进制：<br />1110 0001<br />0011 0000 | <br /> |
| CA_PID | 13 | 0 00001<br />0011 0000<br />(0x0130) |  |  这是一个 13 位字段，指示传输流包的 PID，它应包含 CA 系统的 ECM 或 EMM 信息，如相关 CA_system_ID 所指定。CA_PID 指示的数据包的内容（ECM 或 EMM）是根据 CA_PID 所在的上下文确定的，即TS_program_map_section 或传输流中的 CA 表，或节目流中的 stream_id 字段。 |
| 循环开始 |  |  |  |  |
| private_data_byte | 8 | <br /> | <br /> | descriptor_length只有4字节，前面已经解析完4字节了，没有数据进行循环 |
| 循环结束 |  |  |  |  |

<a name="cyuBz"></a>
##### 解析ES_info 单元流的数据
描述子内容解析完后，开始解析ES_info 单元流的数据<br />单元流数据：<br />02 E2 01 F0 00 04 E2 94 F0 06 0A 04 61 72 61 01 <br />单元流部分数据结构<br />看结构也是循环的结构，可以找到多个单元流

| <br /> | 字段名 | 占位 | 值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| <br /> | 循环开始 |  |  |  |  |
| <br /> | stream_type | 8 | <br /> | <br /> | 8位,指定了节目element的类型 |
| <br /> | reserved | 3 | <br /> | <br /> | 保留 |
| <br /> | elementary_PID | 13 | <br /> | <br /> | 13位,指定了包含program element的TS包的PID |
| <br /> | reserved | 4 | <br /> | <br /> | 保留 |
| <br /> | ES_info_length | 12 | <br /> | <br /> | 头两位为"00" |
| <br /> | 循环开始 |  |  |  | <br /> |
| <br /> | **descriptor（）** |  |  |  | <br /> |
| <br /> | 循环结束 |  |  |  | <br /> |
| <br /> | 循环结束 |  |  |  |  |

<a name="SQJD9"></a>
###### 循环第一个单元流
单元流数据：<br />02 E2 01 F0 00 04 E2 94 F0 06 0A 04 61 72 61 01 

| <br /> | 字段名 | 占位 | 值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| <br /> | stream_type | 8 | 0x02 | 第0字节：<br />02 | 8位,指定了节目element的类型 |
| <br /> | reserved | 3 | 111 （0x07） | 第1~2字节：<br />E2 01<br />二进制：<br />1110 0010<br />0000 0001 | 保留 |
| <br /> | elementary_PID | 13 | 0 0010<br />0000 0001<br />(0x0201) |  | 13位,指定了包含program element的TS包的PID |
| <br /> | reserved | 4 | 1111<br />（0x0F） | 第3~4字节：<br />F0 00<br />二进制：<br />1111 0000<br />0000 0000 | 保留 |
| <br /> | ES_info_length | 12 | 0000<br />0000 0000<br />(0x00) |  | 头两位为"00"<br />ES_info_length=0 |
| <br /> | 循环开始 |  |  |  | <br /> |
| <br /> | **descriptor（）** |  |  |  | 由于<br />ES_info_length==0，所以**descriptor是没有数据的** |
| <br /> | 循环结束 |  |  |  | <br /> |

<a name="UE0JD"></a>
###### 开始第二个单元流数据解析
单元流数据：<br />02 E2 01 F0 00 04 E2 94 F0 06 0A 04 61 72 61 01 

| <br /> | 字段名 | 占位 | 值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
| <br /> | stream_type | 8 | 0x04 | 第0字节：<br />04 | 8位,指定了节目element的类型<br />stream_type=0x04 是 ISO/IEC 13818-3 Audio   |
| <br /> | reserved | 3 | 111 （0x07） | 第1~2字节：<br />E2 94<br />二进制：<br />1110 0010<br />1001 0100 | 保留 |
| <br /> | elementary_PID | 13 | 0 0010<br />1001 0100<br />(0x0294) |  | 13位,指定了包含program element的TS包的PID |
| <br /> | reserved | 4 | 1111<br />（0x0F） | 第3~4字节：<br />F0 06<br />二进制：<br />1111 0000<br />0000 0110 | 保留 |
| <br /> | ES_info_length | 12 | 0000<br />0000 0110<br />(0x06) |  | 头两位为"00"<br />ES_info_length=0x06 |
| <br /> | 循环开始 |  |  |  | <br /> |
| <br /> | **descriptor（）** |  |  |  | 由于<br />ES_info_length==0x06，所以**descriptor是有数据的** |
| <br /> | 循环结束 |  |  |  | <br /> |

开始解析ES_info描述子：<br />数据：<br />02 E2 01 F0 00 04 E2 94 F0 06 0A 04 61 72 61 01<br />descriptor_tag找对应的descriptor结构<br />descriptor_tag：0A，查Program and program element descriptors 表（ISO13818-1）表可知是 ISO_639_language_descriptor  <br />descriptor_length：04， 表示descriptor信息内容是 4 bytes

| ISO_639_language_descriptor   |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x0B | 第0字节：<br />0A | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x0标识<br />ISO_639_language_descriptor   |
| descriptor_length | 8 | 0x02 | 第1字节：<br />04 | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是4个字节 |
| 开始循环(由于一次循环就结束了，所以只有一个描述子) |  |  |  |  |
|  ISO_639_language_code   | 24 | 0x617261 | 第2~4字节<br />61 72 61 | 查uincode字符值得值是字符串: “ara” |
|  audio_type   | 8 | 0x01 | 第5个字节：<br />01 |  audio_type  ==0x01 <br /> Clean effects   |
| 循环结束 |  |  |  |  |
| 


<a name="U8ymt"></a>
## CAT表
条件接收表CAT描述了节目的加密方式，它包含了节目的 EMM 识别PID。 它给出了一个或多个CA系统、EMM流以及与CA相关的特定参数之间的关系。<br />CA描述符既用于规定像EMM这样的系统范围条件接收管理信息，也用于规定像ECM这样的基本流特定信息。

   - 如果一个基本流（Elementary Stream）是加扰的，那么包含该基本流的节目信息PMT中需要一个CA描述符
   - 如果一个TS流中有任何一个系统范围的条件接收管理信息，则条件接收表中应有CA描述符。
<a name="NXlIx"></a>
### CAT表的结构分析
CAT表主要提供关于Bouquet的信息，Bouquet是一个services的集合。<br />    PID = 0x0001<br />    table_id = 0x01<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/29233958/1658652638048-de342f6b-4e32-4d84-8801-b784b85ded11.png#clientId=u70ea7043-f443-4&from=paste&height=273&id=u4f708d9f&originHeight=341&originWidth=824&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47227&status=done&style=none&taskId=u0f3cbb12-e288-46df-be1f-68c2a1bf00c&title=&width=659.2)

- table_id  :这是一个 8 位字段，应按照表 2-26 的规定设置为 0x01。
- section_syntax_indicator:section_syntax_indicator 是一个 1 位字段，应设置为“1”。
-  section_length  :这是一个 12 位字段，前两位应为“00”。 剩余的 10 位指定紧随 section_length 字段开始的部分的字节数，包括 CRC。 该字段的值不得超过 1021 (0x3FD)。
- version_number：这个 5 位字段是整个条件访问表的版本号。 当 CA 表中携带的信息发生变化时，版本号应以 1 模 32 递增。 当 current_next_indicator 设置为“1”时，version_number 应为当前适用的条件访问表的版本号。 当 current_next_indicator 设置为“0”时，version_number 应为下一个适用的条件访问表的版本号。
- current_next_indicator：1 位指示符，当设置为“1”时，指示发送的条件访问表当前适用。 当该位设置为“0”时，表示发送的条件访问表尚不适用，应是下一个生效的条件访问表。
- section_number：这个 8 位字段给出了这个部分的编号。 条件访问表中第一部分的section_number 应为0x00。 条件访问表中每增加一个部分，它就会增加 1。
- last_section_number：这个 8 位字段指定条件访问表的最后一个部分（即 section_number 最高的部分）的编号
- CRC_32：这是一个包含 CRC 值的 32 位字段，在处理完整个条件访问部分后，该值给出了附件 A 中定义的解码器中寄存器的零输出。
<a name="N5s4m"></a>
### CAT包分析
CAT包数据：
```
47 40 01 15 00 01 B0 32 FF FF C7 00 00 09 04 06 
04 05 00 09 1B 05 00 05 01 13 01 20 14 03 04 15 
00 14 03 04 15 10 14 03 04 15 20 14 03 04 15 30 
09 04 17 08 E0 C0 D7 A8 1A C5 FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF 
FF FF FF FF FF FF FF FF FF FF FF FF 
```
| **包头(Package Header)(4 bytes)** | **包数据(Package Data)(184 bytes)** |
| --- | --- |
| 47 40 01 15 | 00 01 B0 32 FF FF C7 00 00 09 04 06 .....FF FF FF |

根据 packet header的结构可以的到：

- PID：0x0001  （CAT PID）
- payload_unit_start_indicator: 1

如果payload_unit_start_indicator=1，在包头后需要除去一个字节才是有效数据.<br />原始数据：<br />00 01 B0 32 FF FF C7 00 00 09 04 06<br />有效数据：<br />       01 B0 32 FF FF C7 00 00 09 04 06 <br />04 05 00 09 1B 05 00 05 01 13 01 20 14 03 04 15 <br />00 14 03 04 15 10 14 03 04 15 20 14 03 04 15 30 <br />09 04 17 08 E0 C0 D7 A8 1A C5
<a name="qKICc"></a>
#### CAT packet 数据分析
| <br /> | 字段名 | 占位（bits） | 值 | 次序 | 说明 |
| --- | --- | --- | --- | --- | --- |
|  | table_id | 8 | 01（0x01） |  01 |  |
|  | section_syntax_indicator | 1 | 1(0x01) | B0 32<br />二进制：<br />1011 0000 0011 0010 |  |
|  | '0' | 1 | 0(0x00) |  |  |
|  | reserved | 2 | 11(0x03) |  |  |
|  | section_length | 12 | 0000 <br />0011 0010<br />（0x0032） |  | 剩余的 10 位指定紧随 section_length 字段开始的部分的字节数，包括 CRC。 该字段的值不得超过 1021 (0x3FD)<br />这里的值是50个字节 |
|  | reserved | 18 | 1111 1111<br />1111 1111<br />11（0x03FFFF） | FF FF C7<br />二进制：<br />1111 1111<br />1111 1111<br />1100 0111 |  |
|  | version_number | 5 | 00 011(0x03) |  |  |
|  | current_next_indicator | 1 | 1(0x01) |  |  |
|  | section_number | 8 | 0x00 | 00 |  |
|  | last_section_number | 8 | 0x00 | 00 |  |
|  | 循环开始 |  |  |  | descriptor数据： |
|  | descriptor（） |  |  |  |  |
|  | 循环结束 |  |  |  |  |
|  | CRC_32 | 32 |  | D7 A8 1A C5 |  |
|  |  |  |  |  |  |

descriptor 的数据：<br />09 04 06 04 05 00 09 1B 05 00 05 01 13 01 20 14 03 04 15 00 14 03 04 15 10 14 03 04 15 20 14 03 04 15 30 09 04 17 08 E0 C0<br />开始循环解析descriptor ：<br />第一个循环：<br />descriptor_tag : 09,查Program and program element descriptors 表（ISO13818-1）表可知是 CA_descriptor<br /> 	  descriptor_length: 04,表示后面4个bytes是descriptor的内容<br />09 04 06 04 05 00

| CA_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x09 | 第0字节：<br />09 | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x09标识<br />CA_descriptor |
| descriptor_length | 8 | 0x04 | 第1字节：<br />04 | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是4个字节 |
| CA_system_ID | 16 | 0000 0100 1110 0001<br />（0x0604） | 第2~3字节：<br />06 04<br />二进制：<br />0000 0110<br />0000 0100 | 这是一个 16 位字段，指示适用于相关 ECM 和/或 EMM 流的 CA 系统类型。其编码是私人定义的，ITU-T 未指定 &#124;ISO/IEC |
| reserved | 3 | 000(0x07) | 第4~5字节：<br />05 00<br />二进制：<br />0000 0101<br />0000 0000 | <br /> |
| CA_PID | 13 | 0 0101<br />0000 0000<br />(0x0500) |  |  这是一个 13 位字段，指示传输流包的 PID，它应包含 CA 系统的 ECM 或 EMM 信息，如相关 CA_system_ID 所指定。CA_PID 指示的数据包的内容（ECM 或 EMM）是根据 CA_PID 所在的上下文确定的，即TS_program_map_section 或传输流中的 CA 表，或节目流中的 stream_id 字段。 |
| 循环开始 |  |  |  |  |
| private_data_byte | 8 | <br /> | <br /> | descriptor_length只有4字节，前面已经解析完4字节了，没有数据进行循环 |
| 循环结束 |  |  |  |  |

第二个循环：<br />descriptor_tag : 09,查Program and program element descriptors 表（ISO13818-1）表可知是 CA_descriptor<br /> 	  descriptor_length: 1B,表示后面27个bytes是descriptor的内容<br /> 09 1B 05 00 05 01 13 01 20 14 03 04 15 00 14 03 04 15 10 14 03 04 15 20 14 03 04 15 30

| CA_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x09 | 第0字节：<br />09 | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x09标识<br />CA_descriptor |
| descriptor_length | 8 | 0x1B | 第1字节：<br />1B | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是27个字节 |
| CA_system_ID | 16 | 0101 0000<br />0000 0000<br />（0x0500） | 第2~3字节：<br />05 00<br />二进制：<br />0101 0000<br />0000 0000 | 这是一个 16 位字段，指示适用于相关 ECM 和/或 EMM 流的 CA 系统类型。其编码是私人定义的，ITU-T 未指定 &#124;ISO/IEC |
| reserved | 3 | 010(0x02) | 第4~5字节：<br />05 01<br />二进制：<br />0101 0000<br />0000 0001 | <br /> |
| CA_PID | 13 | 1 0000<br />0000 0001<br />(0x0101) |  |  这是一个 13 位字段，指示传输流包的 PID，它应包含 CA 系统的 ECM 或 EMM 信息，如相关 CA_system_ID 所指定。CA_PID 指示的数据包的内容（ECM 或 EMM）是根据 CA_PID 所在的上下文确定的，即TS_program_map_section 或传输流中的 CA 表，或节目流中的 stream_id 字段。 |
| 循环开始 |  |  |  |  |
| private_data_byte | 8 | <br /> | 13 01 20 14 03 04 15 00 14 03 04 15 10 14 03 04 15 20 14 03 04 15 30 | <br /> |
| 循环结束 |  |  |  |  |

第三个循环：<br />descriptor_tag : 09,查Program and program element descriptors 表（ISO13818-1）表可知是 CA_descriptor<br /> 	  descriptor_length: 04 ,表示后面4个bytes是descriptor的内容<br /> 09 04 17 08 E0 C0

| CA_descriptor |  |  |  |  |
| --- | --- | --- | --- | --- |
| 字段名 | 占位（bits） | 具体值 | 次序 | 说明 |
| descriptor_tag | 8 | 0x09 | 第0字节：<br />09 | descriptor_tag 是一个 8 位字段，用于标识每个描述符,0x09标识<br />CA_descriptor |
| descriptor_length | 8 | 0x04 | 第1字节：<br />04 | 描述符长度是一个 8 位字段，指定描述符的字节数<br />这里是4个字节 |
| CA_system_ID | 16 | 0001 0111<br />0000 1000<br />（0x1708） | 第2~3字节：<br />17 08<br />二进制：<br />0001 0111<br />0000 1000 | 这是一个 16 位字段，指示适用于相关 ECM 和/或 EMM 流的 CA 系统类型。其编码是私人定义的，ITU-T 未指定 &#124;ISO/IEC |
| reserved | 3 | 111(0x07) | 第4~5字节：<br />E0 C0<br />二进制：<br />1110 0000<br />1100 0000 | <br /> |
| CA_PID | 13 | 0 0000<br />1100 0000<br />(0x00C0) |  |  这是一个 13 位字段，指示传输流包的 PID，它应包含 CA 系统的 ECM 或 EMM 信息，如相关 CA_system_ID 所指定。CA_PID 指示的数据包的内容（ECM 或 EMM）是根据 CA_PID 所在的上下文确定的，即TS_program_map_section 或传输流中的 CA 表，或节目流中的 stream_id 字段。 |
| 循环开始 |  |  |  |  |
| private_data_byte | 8 | <br /> | <br /> | descriptor_length只有4字节，前面已经解析完4字节了，没有数据进行循环 |
| 循环结束 |  |  |  |  |

<a name="B2m5m"></a>
## NIT表
<a name="xTK6R"></a>
### NIT 表概述
NIT描述了数字电视网络中与网络相关的信息，但这个表本身的信息有限，更多的信息是依靠插入表中的描述符来提供的。 NIT常用的描述符有：网络名称描述符（network_name_descriptor）、有线传送系统（cable_delivery_system_descriptor）、业务列表描述符（service_list_descriptor）和链接描述符（linkage_descriptor）
<a name="QGufZ"></a>
### NIT表结构
NIT表主要提供物理网络本身的一些信息。<br />PID = 0x0010<br />table_id：<br />- discribe actual newwork = 0x40<br />- discribe not actual newwork = 0x41<br />网络信息表（NIT）传递了与通过一个给定的网络传输的复用流/TS流的物理结构相关的信息，以及与网络自身特性相关的信息。下表是NIT结构：<br />![22.png](https://cdn.nlark.com/yuque/0/2022/png/29233958/1658807480429-12d20a8f-39a6-47b4-999f-0dbe784434dd.png#clientId=u75c42bd0-4353-4&from=ui&id=u5c21cf81&originHeight=738&originWidth=781&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107221&status=done&style=none&taskId=u438f72ed-d9c6-4802-b8d8-78a74de35e3&title=)
<a name="LrxIz"></a>
### NIT表数据分析
```
47 40 10 1D 00 40 F3 E2 08 00 C5 00 03 F0 1E 5F 
04 00 36 22 75 80 04 00 36 22 75 90 02 08 00 40 
0C 4E 69 6C 65 73 61 74 20 4D 43 4D 45 F3 B7 00 
01 08 00 F0 4B 43 0B 01 17 46 66 00 70 21 02 75 
00 04 41 3C 00 65 01 00 66 01 00 67 01 00 68 01 
00 69 01 00 6A 01 00 6B 01 00 6C 01 00 6D 01 00 
6E 01 00 6F 01 00 70 01 00 78 01 00 79 01 00 7A 
01 00 7D 01 00 7E 01 00 82 02 00 83 02 00 84 02 
00 02 08 00 F0 54 43 0B 01 18 61 74 00 70 36 02 
75 00 02 41 45 00 C9 01 00 CA 01 00 CB 01 00 CC 
01 00 CD 01 00 CE 01 00 CF 01 00 D0 01 00 D3 01 
00 D4 01 00 D5 01 00 D6 01 00 D7 01 
```

<a name="bvcaD"></a>
# SI
PSI只提供了单个TS流的信息，使接收机能够对单个TS流中的不同节目进行解码； 但是，它不能提供多个TS流的相关业务，也不能提供节目的类型、节目名称、开始时间、节目简介等信息。 因此，DVB对PSI进行了扩展，提供了其他不同类型的表，形成了SI。<br />SI定义的表，并不需要全部传输， 其中，SDT、EIT和TDT是必须传输的； 而又以SDT和EIT最为重要，利用这2个表可以构成功能不同的EPG， 如提供节目附加信息、节目分类、节目预定和家长分级控制等。

<a name="N78HT"></a>
## SDT
"SDT描述了业务内容及信息，连接了NIT、EIT和PMT（PSI）"<br />这里所说的业务，即Service；CCTV 1是一个Service(我们说“频道”)，湖南卫视也是一个Service<br />SDT表被切分成业务描述段（service_description_section），由PID为0x0011的TS包传输（BAT段也由PID为0x0011的TS包传输，但table_id不同）。<br />描述现行TS（即包含SDT表的TS）的SDT表的任何段的table_id都为0x42，且具有相同的table_id_extension（transport_stream_id）以及相同的original_network_id。 指向非现行TS的SDT表的任何段的table_id都应取0x46。
