# IDirectFBScreen 接口

`IDirectFBScreen` 接口提供了对DirectFB屏幕对象的操作和管理。

## 信息检索

| 方法               | 描述                           |
| ------------------ | ------------------------------ |
| **GetID**          | 获取唯一的屏幕ID。             |
| **GetDescription** | 获取此屏幕的描述，即能力。     |
| **GetSize**        | 获取屏幕的宽度和高度（像素）。 |

## 显示层

| 方法                  | 描述                         |
| --------------------- | ---------------------------- |
| **EnumDisplayLayers** | 枚举此屏幕的所有现有显示层。 |

## 电源管理

| 方法             | 描述               |
| ---------------- | ------------------ |
| **SetPowerMode** | 设置屏幕电源模式。 |

## 同步

| 方法            | 描述                 |
| --------------- | -------------------- |
| **WaitForSync** | 等待下一次垂直重绘。 |

## 混合器

| 方法                       | 描述                   |
| -------------------------- | ---------------------- |
| **GetMixerDescriptions**   | 获取可用混合器的描述。 |
| **GetMixerConfiguration**  | 获取当前混合器配置。   |
| **TestMixerConfiguration** | 测试混合器配置。       |
| **SetMixerConfiguration**  | 设置混合器配置。       |

## 编码器

| 方法                         | 描述                       |
| ---------------------------- | -------------------------- |
| **GetEncoderDescriptions**   | 获取可用显示编码器的描述。 |
| **GetEncoderConfiguration**  | 获取当前编码器配置。       |
| **TestEncoderConfiguration** | 测试编码器配置。           |
| **SetEncoderConfiguration**  | 设置编码器配置。           |

## 输出

| 方法                        | 描述                 |
| --------------------------- | -------------------- |
| **GetOutputDescriptions**   | 获取可用输出的描述。 |
| **GetOutputConfiguration**  | 获取当前输出配置。   |
| **TestOutputConfiguration** | 测试输出配置。       |
| **SetOutputConfiguration**  | 设置输出配置。       |

## 同步

| 方法              | 描述                   |
| ----------------- | ---------------------- |
| **GetVSyncCount** | 返回当前垂直同步计数。 |