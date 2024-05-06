# IDirectFB 接口

`IDirectFB` 是 DirectFB 的主要接口，可以通过调用 `DirectFBCreate` 获取。它是唯一具有全局创建功能接口。其他接口由这个接口或由它创建的接口创建。

## 硬件能力

可以检索硬件能力，如视频内存的数量或支持的绘图/传输函数和标志的列表。它还提供所有支持的视频模式的枚举。

## 输入设备和显示层

可以通过回调机制枚举当前存在的输入设备和显示层。回调会提供能力和设备或层的 ID。可以通过将设备或层 ID 传递给相应的方法来检索特定输入设备或显示层的接口。

## 表面

可以通过 `CreateSurface` 创建通用目的的表面。这些表面被称为“非屏幕表面”，可以用于精灵或图标。

## 主要表面

主要表面是对获取视觉输出表面的抽象和 API 快捷方式。例如，全屏游戏的整个屏幕就是它们的主要表面。或者，可以通过窗口强制全屏应用程序运行。主要表面也是通过 `CreateSurface` 创建的，但具有特殊的能力 `DSCAPS_PRIMARY`。

## 合作级别

合作级别选择主要表面类型。通过调用 `SetCooperativeLevel`，应用程序可以选择隐式创建的窗口表面和主要层表面（停用窗口栈）。应用程序不需要任何额外的功能就可以在窗口中运行。如果应用程序被迫在窗口中运行，对 `SetCooperativeLevel` 的调用将失败，并返回 `DFB_ACCESSDENIED`。想要“窗口感知”的应用程序不应该在这个错误上退出。

## 视频模式

可以通过 `SetVideoMode` 更改视频模式，这是主层（即在排他合作级别时的屏幕）的大小和深度。没有排他访问权时，`SetVideoMode` 设置隐式创建的窗口的大小。

## 事件缓冲区

可以创建事件缓冲区，并有选项自动附加匹配指定能力的输入设备。如果传递 `DICAPS_NONE`，则创建一个未附加任何内容的事件缓冲区。可以附加事件缓冲区到输入设备和窗口。

## 字体、图像和视频

这些内容由这个接口创建。对于不同类型的内容，有不同的实现方式。在创建时，会自动选择一个合适的实现。

## IDirectFB 方法

| 方法                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| **合作级别、视频模式**   |                                                              |
| `SetCooperativeLevel`    | 将接口置于指定的合作级别。                                   |
| `SetVideoMode`           | 切换当前视频模式（主层）。                                   |
| **硬件能力**             |                                                              |
| `GetDeviceDescription`   | 获取图形设备的描述。                                         |
| `EnumVideoModes`         | 枚举支持的视频模式。                                         |
| **表面和调色板**         |                                                              |
| `CreateSurface`          | 创建与指定描述匹配的表面。                                   |
| `CreatePalette`          | 创建与指定描述匹配的调色板。                                 |
| **屏幕**                 |                                                              |
| `EnumScreens`            | 枚举所有存在的屏幕。                                         |
| `GetScreen`              | 检索特定屏幕的接口。                                         |
| **显示层**               |                                                              |
| `EnumDisplayLayers`      | 枚举所有存在的显示层。                                       |
| `GetDisplayLayer`        | 检索特定显示层的接口。                                       |
| **输入设备**             |                                                              |
| `EnumInputDevices`       | 枚举所有存在的输入设备。                                     |
| `GetInputDevice`         | 检索特定输入设备的接口。                                     |
| `CreateEventBuffer`      | 创建事件缓冲区。                                             |
| `CreateInputEventBuffer` | 创建连接输入设备的事件缓冲区。                               |
| **媒体**                 |                                                              |
| `CreateImageProvider`    | 为指定的文件创建图像提供者。                                 |
| `CreateVideoProvider`    | 创建视频提供者。                                             |
| `CreateFont`             | 根据如何加载字形的描述，从指定的文件加载字体。               |
| `CreateDataBuffer`       | 创建数据缓冲区。                                             |
| **剪贴板**               |                                                              |
| `SetClipboardData`       | 设置剪贴板内容。                                             |
| `GetClipboardData`       | 获取剪贴板内容。                                             |
| `GetClipboardTimeStamp`  | 获取最后一次 `SetClipboardData` 调用的时间戳。               |
| **其他**                 |                                                              |
| `Suspend`                | 挂起 DirectFB，直到调用 `Resume` 之前不允许进行其他 DirectFB 调用。 |
| `Resume`                 | 恢复 DirectFB，只能在调用 `Suspend` 之后调用。               |
| `WaitIdle`               | 等待图形卡空闲，即完成所有绘图/传输函数。                    |
| `WaitForSync`            | 等待下一次垂直重绘。                                         |
| **扩展**                 |                                                              |
| `GetInterface`           | 加载特定接口类型的实现。                                     |
| **表面**                 |                                                              |
| `GetSurface`             | 通过 ID 获取表面。                                           |