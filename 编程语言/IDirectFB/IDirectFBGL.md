# IDirectFBGL 接口

`IDirectFBGL` 接口提供了对DirectFB中的OpenGL上下文的操作和管理。

## 上下文处理

| 方法               | 描述                                 |
| ------------------ | ------------------------------------ |
| **Lock**           | 获取硬件锁。                         |
| **Unlock**         | 释放锁。                             |
| **GetAttributes**  | 查询OpenGL属性。                     |
| **GetProcAddress** | 获取OpenGL函数的地址。               |
| **TextureSurface** | 设置一个表面，以便当前纹理对象使用。 |