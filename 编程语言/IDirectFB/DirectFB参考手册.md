# DirectFB 参考手册 - 接口部分

DirectFB 提供了一系列接口，用于执行图形和输入相关的操作。以下是一些主要的 DirectFB 接口：

[https://gongg08.gitee.io/directfb_doc/](https://gongg08.gitee.io/directfb_doc/)

## [IDirectFB](./IDirectFB.md)

- **主要接口**：可以调用 `DirectFBCreate` 来检索。它是唯一具有全局创建设施的接口。其他接口都是由这个接口或者由它创建的接口来创建的。

## [IDirectFBSurface](./IDirectFBSurface.md)

- **表面操作**：用于创建和操作图形表面（Surface），如非屏幕表面（offscreen surfaces）和主表面（primary surface）。

## [IDirectFBWindow](./IDirectFBWindow.md)

- **窗口操作**：用于创建和管理窗口，包括获取窗口的表面和设置窗口的属性。

## [IDirectFBEventBuffer](./IDirectFBEventBuffer.md)

- **事件缓冲区**：用于创建和管理事件缓冲区，可以与输入设备或窗口关联。

## [IDirectFBPalette](./IDirectFBPalette.md)

- **调色板操作**：用于操作颜色调色板，包括设置和获取调色板条目。

## [IDirectFBImageProvider](./IDirectFBImageProvider.md)

- **图像提供者**：用于加载和解码图像数据。

## [IDirectFBVideoProvider](./IDirectFBVideoProvider.md)

- **视频提供者**：用于播放和控制视频内容。

## [IDirectFBFont](./IDirectFBFont.md)

- **字体操作**：用于加载和使用字体。

## [IDirectFBDataBuffer](./IDirectFBDataBuffer.md)

- **数据缓冲区**：用于处理二进制数据缓冲区，如读取和写入数据。

## [IDirectFBGL](./IDirectFBGL.md)

- **OpenGL 上下文**：提供了对 DirectFB 中 OpenGL 上下文的操作。

## [IDirectFBGL2](./IDirectFBGL2.md)

- **OpenGL ES 2.x 上下文**：用于管理 OpenGL ES 2.x 渲染上下文。

## [IDirectFBScreen](./IDirectFBScreen.md)

- **屏幕操作**：用于获取屏幕的属性和控制屏幕的显示设置。

## [IDirectFBWindows](./IDirectFBWindow.md)

- **窗口监视**：允许应用程序注册窗口监视器，以接收有关窗口变化的通知。

这些接口是 DirectFB API 的核心部分，它们提供了创建和管理图形内容、处理用户输入和播放多媒体内容的能力。

# DirectFB 参考手册 - 函数类型部分

DirectFB 定义了一系列函数类型，主要用于回调函数。以下是一些主要的函数类型：

## DFBEnumerationResult

- **描述**：枚举回调函数的返回值。用于控制枚举过程的继续或停止。

## DFBEventFilter

- **描述**：事件过滤器回调函数。用于在事件传递给目标之前检查事件。

## DFBEventHandler

- **描述**：事件处理程序回调函数。用于处理特定事件。

## DFBFrameCallback

- **描述**：帧回调函数。用于在每帧开始或结束时执行特定操作。

## DFBInputDeviceCallback

- **描述**：输入设备回调函数。用于响应输入设备的添加或移除。

## DFBLayerCallback

- **描述**：层回调函数。用于响应显示层的添加或移除。

## DFBScreenCallback

- **描述**：屏幕回调函数。用于响应屏幕的添加或移除。

## DFBScreenMixerCallback

- **描述**：屏幕混音器回调函数。用于响应屏幕混音器的添加或移除。

## DFBSurfaceCallback

- **描述**：表面回调函数。用于响应表面的添加或移除。

## DFBVideoProviderCallback

- **描述**：视频提供者回调函数。用于响应视频流的添加或移除。

## DFBWindowCallback

- **描述**：窗口回调函数。用于响应窗口的添加或移除。

这些函数类型通常作为参数传递给 DirectFB 的某些函数，用于实现事件驱动的编程模型。开发者可以根据需要实现这些回调函数，以处理 DirectFB 运行时的各种事件和状态变化。

# DirectFB 参考手册 - 枚举类型部分

DirectFB 定义了一系列枚举类型，用于指定不同的配置选项和行为。以下是一些主要的枚举类型：

## DFBAccelerationMask

- **描述**：加速功能的掩码。

## DFBBoolean

- **描述**：布尔值。

## DFBColorAdjustmentFlags

- **描述**：定义 `DFBColorAdjustment` 结构体中哪些字段是有效的标志。

## DFBColorKeyPolarity

- **描述**：颜色键极性。

## DFBCooperativeLevel

- **描述**：控制顶层接口行为的合作级别，例如在 `SetVideoMode` 或 `CreateSurface` 函数中。

## DFBDataBufferDescriptionFlags

- **描述**：定义 `DFBDataBufferDescription` 结构体中哪些字段是有效的标志。

## DFBDisplayAspectRatio

- **描述**：显示的纵横比。

## DFBDisplayLayerBackgroundMode

- **描述**：背景模式，定义窗口栈重绘时如何擦除/初始化区域。

## DFBDisplayLayerBufferMode

- **描述**：显示层缓冲区模式。

## DFBDisplayLayerCapabilities

- **描述**：显示层的能力。

## DFBDisplayLayerConfigFlags

- **描述**：显示层配置标志。

## DFBDisplayLayerCooperativeLevel

- **描述**：处理访问权限的合作级别。

## DFBDisplayLayerOptions

- **描述**：用于启用某些能力，如闪烁过滤或颜色键。

## DFBDisplayLayerSourceCaps

- **描述**：显示层源的能力。

## DFBDisplayLayerTypeFlags

- **描述**：显示层的基本分类类型。值可以组合使用。

## DFBEnumerationResult

- **描述**：枚举回调函数的返回值。

## DFBEventClass

- **描述**：事件类。

## DFBFontAttributes

- **描述**：描述如何加载字体的标志。

## DFBFontDescriptionFlags

- **描述**：定义 `DFBFontDescription` 结构体中哪些字段是有效的标志。

## DFBFrameTimeConfigFlags

- **描述**：帧时间配置标志。

## DFBGL2ContextCapabilities

- **描述**：上下文的能力。

## DFBGL2ContextDescriptionFlags

- **描述**：上下文描述的标志。

## DFBImageCapabilities

- **描述**：图像的能力。

## DFBInputDeviceAxisIdentifier

- **描述**：轴标识符（例如鼠标或游戏手柄的索引）。

## DFBInputDeviceAxisInfoFlags

- **描述**：输入设备轴信息的标志。

## DFBInputDeviceButtonIdentifier

- **描述**：按钮标识符（例如鼠标或游戏手柄按钮的索引）。

## DFBInputDeviceButtonMask

- **描述**：指定当前哪些按钮被按下的标志。

## DFBInputDeviceButtonState

- **描述**：指定按钮当前是否被按下。

## DFBInputDeviceCapabilities

- **描述**：基本输入设备特性。

## DFBInputDeviceConfigFlags

- **描述**：输入设备配置标志。

## DFBInputDeviceKeyIdentifier

- **描述**：DirectFB 键标识符（用于基本映射）。

## DFBInputDeviceKeyState

- **描述**：指定键当前是否被按下。

## DFBInputDeviceKeySymbol

- **描述**：DirectFB 键符号（用于高级映射）。

## DFBInputDeviceKeyType

- **描述**：DirectFB 键类型（用于高级映射）。

## DFBInputDeviceKeymapSymbolIndex

- **描述**：组和级别作为符号数组的索引。

## DFBInputDeviceLockState

- **描述**：指定当前哪些键锁处于活动状态的标志。

## DFBInputDeviceModifierKeyIdentifier

- **描述**：DirectFB 修改键标识符（用于高级映射）。

## DFBInputDeviceModifierMask

- **描述**：指定当前哪些修饰键被按下的标志。

## DFBInputDeviceTypeFlags

- **描述**：输入设备的基本分类类型。值可以组合使用。

## DFBInputEventFlags

- **描述**：定义输入事件中哪些附加（可选）字段是有效的标志。

## DFBInputEventType

- **描述**：输入事件的类型。

## DFBPaletteCapabilities

- **描述**：调色板的能力。

## DFBPaletteDescriptionFlags

- **描述**：定义 `DFBPaletteDescription` 结构体中哪些字段是有效的标志。

## DFBScreenCapabilities

- **描述**：屏幕的能力。

## DFBScreenEncoderCapabilities

- **描述**：显示编码器的能力。

## DFBScreenEncoderConfigFlags

- **描述**：显示编码器配置的标志。

## DFBScreenEncoderFrequency

- **描述**：输出信号的频率。

## DFBScreenEncoderPictureFraming

- **描述**：编码器画面传输方式。详见 HDMI 规范 1.4a - 3D 信号部分的提取。

## DFBScreenEncoderScanMode

- **描述**：扫描模式。

## DFBScreenEncoderTVStandards

- **描述**：电视标准。

## DFBScreenEncoderTestPicture

- **描述**：测试画面模式。

## DFBScreenEncoderType

- **描述**：显示编码器的类型。

## DFBScreenMixerCapabilities

- **描述**：混音器的能力。

## DFBScreenMixerConfigFlags

- **描述**：混音器配置的标志。

## DFBScreenMixerTree

- **描述**：（子）树选择。

## DFBScreenOutputCapabilities

- **描述**：输出的能力。

## DFBScreenOutputConfigFlags

- **描述**：屏幕输出配置的标志。

## DFBScreenOutputConnectors

- **描述**：输出连接器的类型。

## DFBScreenOutputResolution

- **描述**：屏幕输出分辨率。

## DFBScreenOutputSignals

- **描述**：输出信号的类型。

## DFBScreenOutputSlowBlankingSignals

- **描述**：慢速消隐信号的类型。

## DFBScreenPowerMode

- **描述**：屏幕电源模式。

## DFBStreamCapabilities

- **描述**：音视频流的能力。

## DFBStreamFormat

- **描述**：音频流的类型。

## DFBSurfaceBlendFunction

- **描述**：用于源和目标混合的混合函数。

## DFBSurfaceBlittingFlags

- **描述**：控制 blitting 命令的标志。

## DFBSurfaceCapabilities

- **描述**：表面的能力。

## DFBSurfaceColorSpace

- **描述**：表面颜色使用的颜色空间。

## DFBSurfaceDescriptionFlags

- **描述**：定义 `DFBSurfaceDescription` 结构体中哪些字段是有效的标志。

## DFBSurfaceDrawingFlags

- **描述**：控制绘图命令的标志。

## DFBSurfaceEventType

- **描述**：表面事件类型 - 也可以作为事件过滤器的标志使用。

## DFBSurfaceFlipFlags

- **描述**：控制 `IDirectFBSurface::Flip()` 行为的翻转标志。

## DFBSurfaceHintFlags

- **描述**：优化分配、格式选择等的提示标志。

## DFBSurfaceLockFlags

- **描述**：定义数据访问类型的的标志。这对于表面交换管理很重要。

## DFBSurfaceMaskFlags

- **描述**：控制通过 `IDirectFBSurface::SetSourceMask()` 设置的表面掩码的标志。

## DFBSurfacePatternMode

- **描述**：可用的图案模式。

## DFBSurfacePixelFormat

- **描述**：表面的像素格式。

## DFBSurfacePorterDuffRule

- **描述**：可用的 Porter/Duff 规则。

## DFBSurfaceRenderOptions

- **描述**：绘图和 blitting 操作的选项。对于加速不是强制性的。

## DFBSurfaceRopCode

- **描述**：可用的 Rop 代码。

## DFBSurfaceStereoEye

- **描述**：立体眼睛缓冲。

## DFBSurfaceTextFlags

- **描述**：控制文本布局的标志。

## DFBTriangleFormation

- **描述**：从顶点列表构建三角形的方式。

## DFBVideoProviderAudioUnits

- **描述**：允许音频单元选择的标志。

## DFBVideoProviderCapabilities

- **描述**：`IDirectFBVideoProvider` 的信息。

## DFBVideoProviderEventDataSubType

- **描述**：视频提供者事件类型 - 也可以作为事件过滤器的标志使用。

## DFBVideoProviderEventType

- **描述**：视频提供者事件类型 - 也可以作为事件过滤器的标志使用。

- # DirectFB 参考手册 - 枚举类型部分（续）

  ## DFBVideoProviderPlaybackFlags

  - **描述**：控制 `IDirectFBVideoProvider` 播放模式的标志。

  ## DFBVideoProviderStatus

  - **描述**：提供有关 `IDirectFBVideoProvider` 状态的信息。

  ## DFBWindowCapabilities

  - **描述**：窗口可以拥有的能力。

  ## DFBWindowConfigFlags

  - **描述**：窗口配置的标志。

  ## DFBWindowCursorFlags

  - **描述**：窗口光标的标志。

  ## DFBWindowDescriptionFlags

  - **描述**：定义 `DFBWindowDescription` 结构体中哪些字段是有效的标志。

  ## DFBWindowEventFlags

  - **描述**：窗口事件的标志。

  ## DFBWindowEventType

  - **描述**：窗口事件类型 - 也可以作为事件过滤器的标志使用。

  ## DFBWindowGeometryMode

  - **描述**：窗口几何模式。

  ## DFBWindowKeySelection

  - **描述**：键选择定义了窗口在获得焦点时过滤键的模式。

  ## DFBWindowOptions

  - **描述**：控制窗口外观和行为的标志。

  ## DFBWindowRelation

  - **描述**：窗口关系。

  ## DFBWindowStackingClass

  - **描述**：堆叠类限制了窗口的堆叠顺序。

  ## DFBWindowStateFlags

  - **描述**：窗口状态的标志。

  ## DIRenderCallbackResult

  - **描述**：图像提供者使用的回调结果标志。

  ## DIRenderFlags

  - **描述**：图像提供者使用的标志。

  ## WaterAttributeFlags

  - **描述**：每个属性的标志 [8]。

  ## WaterAttributeType

  - **描述**：属性包括渲染上下文的所有设置 [8]。

  ## WaterBlendMode

  - **描述**：混合模式 [8]。

  ## WaterElementFlags

  - **描述**：每个元素的标志 [12]。

  ## WaterElementType

  - **描述**：基本和高级元素的复合类型，带有附加信息 [16]。

  ## WaterFillRule

  - **描述**：填充规则 [4]。

  ## WaterGradientFlags

  - **描述**：渐变标志 [4]。

  ## WaterGradientType

  - **描述**：渐变类型 [4]。

  ## WaterLineCapStyle

  - **描述**：线条端点的风格 [4]。

  ## WaterLineJoinStyle

  - **描述**：线条连接点的风格 [4]。

  ## WaterOperator

  - **描述**：应用值的方式 [4]。

  ## WaterPaintOptions

  - **描述**：关于绘图/填充操作源的选项 [16]。

  ## WaterPatternFlags

  - **描述**：图案标志 [4]。

  ## WaterPatternType

  - **描述**：图案类型 [4]。

  ## WaterQualityLevel

  - **描述**：质量级别 [4]。

  ## WaterRenderMode

  - **描述**：通常适用于渲染或目标的选项 [16]。

  ## WaterScalarType

  - **描述**：标量类型 [4]。

  ## WaterShapeFlags

  - **描述**：每个形状的标志 [8]。

  ## WaterTileMode

  - **描述**：图案和掩码的平铺模式 [4]。

  ## WaterTransformFlags

  - **描述**：选择预定义或自由变换的标志及其他 [8]。

  ## WaterTransformType

  - **描述**：常见的变换类型 [16]。